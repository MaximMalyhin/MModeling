## lab04 presentation
НПИ 02-20
Малыхин Максим

**Вариант 20**

# Цель #
---
Смоделировать работу гармонического осциллятора по заданному уравнению

# Задача #
---
Постройте фазовый портрет гармонического осциллятора и решение уравнения
гармонического осциллятора для следующих случаев
1. Колебания гармонического осциллятора без затуханий и без действий внешней
силы
x''+0.8x=0

На интервале
[0;41] (шаг 0.05) с начальными условиями
x0=0.4
y0=0.3

# Решение #
---
Преобразуем уравнение в два дифференциальных уравнения первого порядка

x''+0.8x=0
->
x'= y
y'=-0.8x

# Код на Julia #

using Plots
using DifferentialEquations

t0 = 0
tmax = 41
t = collect(LinRange(t0, tmax, 1200)) 
tspan = (t0, tmax)
x0 = 0.4
y0 = 0.3
u0 = [x0; y0]

#без затуханий 
#без внешней силы
w = 0.8
function syst(dy, y, p, t)
    dy[1] = y[2]
    dy[2] = -w*y[1]
end

prob = ODEProblem(syst, u0, tspan)
sol = solve(prob, saveat=t)

plot(sol, idxs=(2), color=:blue)
savefig("C:\\jul\\1.png")
plot(sol, idxs=(1, 2), color=:red)
savefig("C:\\jul\\2.png")

#c затуханием
#без действий внешнейсилы
g = 0.8
w = 0.4
function syst(dy, y, p, t)
    dy[1] = y[2]
    dy[2] = -g*y[2]-w*y[1]
end

prob = ODEProblem(syst, u0, tspan)
sol = solve(prob, saveat=t)

plot(sol, idxs=(2), color=:blue)
savefig("C:\\jul\\3.png")
plot(sol, idxs=(1, 2), color=:red)
savefig("C:\\jul\\4.png")

#c затуханием 
#c внешней силой
g = 1
w = 5
function F(t)
    return cos(5*t)
end
function syst(dy, y, p, t)
    dy[1] = y[2]
    dy[2] = -g*y[2]-w*y[1] + F(t)
end

prob = ODEProblem(syst, u0, tspan)
sol = solve(prob, saveat=t)

plot(sol, idxs=(2), color=:blue)
savefig("C:\\jul\\5.png")
plot(sol, idxs=(1, 2), color=:red)
savefig("C:\\jul\\6.png")

# Код на OpenModelica #
model one
parameter Real w = 0.8;
Real x(start=1);
Real y(start=2);
equation
  der(x) = y;
  der(y) = -w*x;  
end one;
!["Первый случай"](/screens/one.png)

model two
parameter Real w = 0.8;
parameter Real g = 0.4;
Real x(start=1);
Real y(start=2);
equation
  der(x) = y;
  der(y) = -g*y - w*x;
end two;
!["Второй случай"](/screens/two.png)

model three
parameter Real w = 1;
parameter Real g = 5;
Real x(start=1);
Real y(start=2);
equation
  der(x) = y;
  der(y) = -g*y - w*x + cos(5*time);
end three;
!["Третий случай"](/screens/three.png)

# Вывод #   
---
Я смоделировал работу гармонического осциллятора по заданному уравнению

