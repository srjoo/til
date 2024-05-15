
# 인터프리터(Interpreter)

- 어떤 언어의 대해, 그 언어의 문법에 대한 표현을 정의하면서 그것(표현)을 사용하여 해당 언어로 기술된 문장을 해석하는 해석자를 함께 정의
- 자주 등장하는 문제를 간단한 언어로 정의하고 재사용하는 패턴
> 컴포지트 패턴과 상당히 유사하다

# 장점
- 자주 등장하는 문제 패턴을 언어와 문법으로 정의할 수 있다
- 기존 코드를 변경하지 않고 새로운 Expression을 만들 수 있다

# 단점
- 복잡한 문법을 표현하려면 Expression와 Parser가 복잡해진다

# 예제
간단한 계산식을 계산하는 계산기 구현
```java
interface Expression {
    int interpret();
}

class Number implements Expression {
    private int number;

    public Number(int number) {
        this.number = number;
    }

    public int interpret() {
        return number;
    }
}

class Add implements Expression {
    private Expression leftOperand;
    private Expression rightOperand;

    public Add(Expression left, Expression right) {
        leftOperand = left;
        rightOperand = right;
    }

    public int interpret() {
        return leftOperand.interpret() + rightOperand.interpret();
    }
}

class Subtract implements Expression {
    private Expression leftOperand;
    private Expression rightOperand;

    public Subtract(Expression left, Expression right) {
        leftOperand = left;
        rightOperand = right;
    }

    public int interpret() {
        return leftOperand.interpret() - rightOperand.interpret();
    }
}

class CalculatorContext {
    private String expression;

    public CalculatorContext(String expression) {
        this.expression = expression;
    }

    public String getExpression() {
        return expression;
    }
}

public class Main {
    public static void main(String[] args) {
        // Context 객체 생성
        CalculatorContext context = new CalculatorContext("5 + 3 - 2");

        // 문법 해석
        Expression expression = parse(context);

        // 결과 출력
        System.out.println(context.getExpression() + " = " + expression.interpret());
    }

    private static Expression parse(CalculatorContext context) {
        String[] tokens = context.getExpression().split(" ");
        Stack<Expression> stack = new Stack<>();

        for (int i = 0; i < tokens.length; i++) {
            if (tokens[i].equals("+")) {
                Expression left = stack.pop();
                Expression right = new Number(Integer.parseInt(tokens[++i]));
                stack.push(new Add(left, right));
            } else if (tokens[i].equals("-")) {
                Expression left = stack.pop();
                Expression right = new Number(Integer.parseInt(tokens[++i]));
                stack.push(new Subtract(left, right));
            } else {
                stack.push(new Number(Integer.parseInt(tokens[i])));
            }
        }

        return stack.pop();
    }
}
```

# 참고

|내용|URL|
|:---|:---|
|인터프리터 패턴|https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%84%B0_%ED%8C%A8%ED%84%B4|
|[Design Pattern] Interpreter(인터프리터) 패턴이란?|https://shan0325.tistory.com/29|