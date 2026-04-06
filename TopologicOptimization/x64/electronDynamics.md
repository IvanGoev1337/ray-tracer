# Структура 
Программа ставит целью расчёт движения электронов в электрических полях<br>
источниками этого поля могут быть как неподвижные протоны, так и внешние<br>
электрические поля, поэтомы вся симуляция представляет собой набор<br> электронов, характеризующиеся положением и скоростью, а также протоны<br>
с внишними полями.<br>
# Пример
Для начала необходимо задать положения и скорости всех электронов в<br>
симуляции:<br>
```cpp
const unsigned int N_e = 64;
electron ans[N_e];
for (int i = 0;i < N_e;i++) {
    startPos[i].set(int(i / 8) * dx + 3, i % 8 * dx, 0);
}
for (unsigned int i = 0;i < N_e;i++) {
    startV[i].set(0, 1, 0);
}
for (int i = 0;i < N_e;i++) {
    ans[i].set(startPos[i], startV[i]);
}
```
Затем запустить симуляцию, которая предсавляет собой решение<br>
системы ОДУ составленых из 2 закона Ньютона, методом Рунге-Кутта<br>
нулевого порядка, реализовано это следующим образом:<br>
```cpp
for (unsigned int i = 0;i < t_limit;i++) {
    electricFieldCalculate(ans, env, force);
    //force[0].print();
    for (int j = 0;j < N_e;j++) {
        dataFile << ans[j].to_string() << endl;
        ans[j].newStep(dt, force[j]);
    }
    dataFile << "new step" << endl;
}
dataFile.close();
std::cout << "Hello World!\n";
```
На каждом новом шаге программа вычисляет силы взаимодействия <br>
электронов с полем:<br>
```cpp
static void electricFieldCalculate(electron* A, proton* p, vector* res) {
    vector field[N_e][N_e];
    for (int i = 0;i < N_e;i++) {  
        for (int j = 0;j < N_e;j++) {
            if (i == j) {
                field[i][j].zero();
                continue;
            }
            field[i][j] = A[i].getElectricField(A[j].position);
        }
    }
    vector resTemp[N_e];
    for (int i = 0;i < N_e;i++) {
        resTemp[i].zero();
    }
    for (int i = 0;i < N_e;i++) {
        resTemp[i] = enviromentField(A[i].position, p) * q_e;
        for (int j = 0;j < N_e;j++) {
            resTemp[i] = resTemp[i] + field[i][j];
        }
    }
    for (int i = 0;i < N_e;i++) res[i] = resTemp[i];
}
```
Для анализа результатов расчёта визуализируем их с помощью библиотеки<br> Python matplotlib, визуализироваться будет расчёт ансамбля из 64 <br>
электронов,расположенных в виде квадрата со стороной равной 8 и <br>
скоростями 1 м/с, напраленные вдоль оси OY, центр квадрата смещён <br>
на 3 в право, помимо электронов присутствует 1 протон в центре координат.<br>
