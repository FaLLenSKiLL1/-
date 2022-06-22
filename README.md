# -
Код позволяет производить сложение и умножение точек на эллиптической кривой
Для сложения двух одинаковых точек рекомендую использовать умножение этой точки, так как python не любит делить на 0, и при совершении этого действия вселенная схлопнется :/

def elden_ring(a, b): # Алгоритм Евклида
    if b == 0:
        return a, 1, 0
    else:
        d, x, y = elden_ring(b, a % b)
        return d, y, x - y * (a // b)

def reverse(n, mod): # Вычисление обратного элемента в кольце по моду
    b = elden_ring(n, mod)
    a = (mod * b[1]) % n
    n = 0 if b[0] > 1 else b[1]
    if n < 0: # Исключение, при котором в ответе получилось отрицательное число
        n = mod + n
    return n

def _point_addition(x1, y1, x2, y2, P): # Сложение двух точек эллиптической кривой
    ol = y2 - y1 #Числитель при вычислении λ
    lo = x2 - x1 #Знаменатель при вычислении λ
    print(f"λ = ({y2} - {y1}/{x2} - {x1})mod{P} = ({y2 - y1}/{x2 - x1})mod{P} = ")
    print(f"({y2 - y1}*{x2 - x1}^-1)mod{P} = ({y2 - y1}*{reverse(lo, P)})mod{P} = ")
    print(f"({(y2 - y1) * (reverse(lo, P))})mod{P} = ")
    lo = reverse(lo, P)
    _lambd = (lo * ol) % P
    print(_lambd)
    x3 = ((_lambd ** 2) - x1 - x2) % P
    print(f"x3 = (({_lambd}^2) - {x1} - {x2})mod{P} = ({_lambd**2} - {x1 + x2})mod{P} = ")
    print(f"{_lambd**2 - (x1 + x2)}mod{P} = ", x3)
    y3 = (-y1 + (_lambd * (x1 - x3))) % P
    print(f"y3 = ({-y1} + ({_lambd} * ({x1} - {x3})))mod{P} = ({-y1} + ({_lambd} * ({x1 - x3})))mod{P} = ")
    print(f"({-y1} + ({_lambd * (x1 - x3)}))mod{P} = ({-y1} + ({_lambd * (x1 - x3)}))mod{P} = ")
    print(f"{-y1 + (_lambd * (x1 - x3))}mod{P} = ",y3)
    return print(f"{(x3, y3)}")

def _point_multiplication(x1,y1,P,a): # Умножение точки эллиптической кривой
    ol=(3*x1**2+a) #Числитель при вычислении λ
    lo=((2*y1)%P)  #Знаменатель при вычислении λ
    print(f"λ = (3*{x1}^2+{a}/(2*{y1}))mod{P} = (3*{x1**2}+{a}/({2*y1}))mod{P} =  ")
    print(f"({3*x1**2+a}/({2 * y1}))mod{P} = ({3 * x1 ** 2 + a}*({2 * y1}^-1)mod{P} = ")
    print(f"({3 * x1 ** 2 + a}*{reverse(lo,P)})mod{P} = " )
    lo = reverse(lo, P)
    _lambd = (lo * ol) % P
    print(_lambd)
    x3=((_lambd**2)-(2*x1))%P
    print(f"x3=(({_lambd}^2)-(2*{x1}))mod{P} = (({_lambd**2})-({2*x1}))mod{P} = ")
    print(f"({_lambd ** 2 - 2 * x1})mod{P} = ",x3)
    y3=(-y1+(_lambd*(x1-x3)))%P
    print(f"y3=({-y1}+({_lambd}*({x1}-{x3})))mod{P} = ({-y1}+({_lambd}*({x1-x3})))mod{P} = ")
    print(f"({-y1}+{_lambd * (x1 - x3)})mod{P} = ({-y1 +_lambd * (x1 - x3)})mod{P} = ",y3)
    return print(f"{x3,y3}")


if __name__ == '__main__':
    O = int(input("Для выхода введите 0, для продолжения - любое число: ")) # Для удобства ввода данных, чтобы постоянно не тыкать запустить :)
    while O != 0 :
        W = int(input("Введите 1 для сложения точек или 2 для умножения: ")) # Переключатель "режимов"
        if W == 2:
            _point_multiplication(int(input("Введите x: ")),int(input("Введите y: ")),int(input("Введите P: ")),int(input("Введите a: "))) #Входные дынные для умножения
        elif W == 1:
            _point_addition(int(input("Введите x1: ")),int(input("Введите y1: ")),int(input("Введите x2: ")),int(input("Введите y2: ")),int(input("Введите P: "))) #Входные дынные для сложения
