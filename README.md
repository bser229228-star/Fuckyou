```mermaid
flowchart TD
    Start([Начало]) --> SetLocale[setlocale LC_ALL Russian]
    SetLocale --> InputN[/Вывод: Введите размер n/]
    InputN --> ReadN[/Ввод n/]
    ReadN --> CheckN{n <= 0?}
    
    CheckN -->|Да| Error[/Ошибка: n > 0/]
    Error --> Stop1([Конец return 1])
    
    CheckN -->|Нет| CreateMatrix[<b>createMatrix(n)</b><br/>Предопределённый процесс]
    
    CreateMatrix --> FillMatrix[<b>fillMatrix(A, n)</b><br/>Предопределённый процесс]
    FillMatrix --> PrintMatrix[<b>printMatrix(A, n)</b><br/>Предопределённый процесс]
    PrintMatrix --> Task1[<b>sumRowsWithoutNegatives(A, n)</b><br/>Предопределённый процесс]
    Task1 --> Task2[<b>minSumOfParallelDiagonals(A, n)</b><br/>Предопределённый процесс]
    Task2 --> DeleteMatrix[<b>deleteMatrix(A, n)</b><br/>Предопределённый процесс]
    DeleteMatrix --> Stop2([Конец return 0])

    %% Подробное описание createMatrix
    subgraph createMatrix_detail [createMatrix]
        CM_Start([Начало]) --> CM_Alloc1[matrix = new int*[n]]
        CM_Alloc1 --> CM_LoopStart{i = 0}
        CM_LoopStart --> CM_Cond{i < n?}
        CM_Cond -->|Да| CM_Alloc2[matrix[i] = new int[n]]
        CM_Alloc2 --> CM_Increment[i++]
        CM_Increment --> CM_Cond
        CM_Cond -->|Нет| CM_Return[return matrix]
        CM_Return --> CM_End([Конец])
    end

    %% Подробное описание fillMatrix
    subgraph fillMatrix_detail [fillMatrix]
        FM_Start([Начало]) --> FM_Out[/Вывод: Введите элементы n×n/]
        FM_Out --> FM_i{i = 0}
        FM_i --> FM_iCond{i < n?}
        FM_iCond -->|Да| FM_j{j = 0}
        FM_j --> FM_jCond{j < n?}
        FM_jCond -->|Да| FM_Prompt[/Вывод: A[i][j] = /]
        FM_Prompt --> FM_Read[/Ввод matrix[i][j]/]
        FM_Read --> FM_jInc[j++]
        FM_jInc --> FM_jCond
        FM_jCond -->|Нет| FM_iInc[i++]
        FM_iInc --> FM_iCond
        FM_iCond -->|Нет| FM_End([Конец])
    end

    %% Подробное описание printMatrix
    subgraph printMatrix_detail [printMatrix]
        PM_Start([Начало]) --> PM_OutTitle[/Вывод: Сформированная матрица/]
        PM_OutTitle --> PM_i{i = 0}
        PM_i --> PM_iCond{i < n?}
        PM_iCond -->|Да| PM_j{j = 0}
        PM_j --> PM_jCond{j < n?}
        PM_jCond -->|Да| PM_OutElem[/Вывод matrix[i][j] с табуляцией/]
        PM_OutElem --> PM_jInc[j++]
        PM_jInc --> PM_jCond
        PM_jCond -->|Нет| PM_Newline[/Вывод: \\n/]
        PM_Newline --> PM_iInc[i++]
        PM_iInc --> PM_iCond
        PM_iCond -->|Нет| PM_End([Конец])
    end

    %% Подробное описание sumRowsWithoutNegatives
    subgraph sumRows_detail [sumRowsWithoutNegatives]
        SR_Start([Начало]) --> SR_Init[foundAny = false]
        SR_Init --> SR_OutTitle[/Вывод: === ЗАДАНИЕ 1 ===/]
        SR_OutTitle --> SR_i{i = 0}
        SR_i --> SR_iCond{i < n?}
        SR_iCond -->|Да| SR_InitRow[hasNegative = false<br/>rowSum = 0]
        SR_InitRow --> SR_j{j = 0}
        SR_j --> SR_jCond{j < n?}
        SR_jCond -->|Да| SR_Add[rowSum += matrix[i][j]]
        SR_Add --> SR_CheckNeg{matrix[i][j] < 0?}
        SR_CheckNeg -->|Да| SR_SetNeg[hasNegative = true]
        SR_SetNeg --> SR_jInc[j++]
        SR_CheckNeg -->|Нет| SR_jInc
        SR_jInc --> SR_jCond
        SR_jCond -->|Нет| SR_CheckRow{!hasNegative?}
        SR_CheckRow -->|Да| SR_Print[/Вывод: Строка i+1: rowSum/]
        SR_Print --> SR_SetFound[foundAny = true]
        SR_SetFound --> SR_iInc[i++]
        SR_CheckRow -->|Нет| SR_iInc
        SR_iInc --> SR_iCond
        SR_iCond -->|Нет| SR_CheckAny{!foundAny?}
        SR_CheckAny -->|Да| SR_NoRows[/Вывод: Нет строк без отрицательных/]
        SR_NoRows --> SR_End([Конец])
        SR_CheckAny -->|Нет| SR_End
    end

    %% Подробное описание minSumOfParallelDiagonals
    subgraph minSum_detail [minSumOfParallelDiagonals]
        MS_Start([Начало]) --> MS_Init[first = true<br/>minSum = 0]
        MS_Init --> MS_OutTitle[/Вывод: === ЗАДАНИЕ 2 ===/]
        MS_OutTitle --> MS_ColLoop[startCol = 0]
        MS_ColLoop --> MS_ColCond{startCol < n?}
        MS_ColCond -->|Да| MS_Sum1[sum = 0<br/>i = 0<br/>j = startCol]
        MS_Sum1 --> MS_While1{<b>пока</b> i < n И j < n?}
        MS_While1 -->|Да| MS_Add1[sum += matrix[i][j]<br/>i++<br/>j++]
        MS_Add1 --> MS_While1
        MS_While1 -->|Нет| MS_CheckFirst{first?}
        MS_CheckFirst -->|Да| MS_SetMin1[minSum = sum<br/>first = false]
        MS_SetMin1 --> MS_ColInc[startCol++]
        MS_CheckFirst -->|Нет| MS_Compare1{sum < minSum?}
        MS_Compare1 -->|Да| MS_Update1[minSum = sum]
        MS_Update1 --> MS_ColInc
        MS_Compare1 -->|Нет| MS_ColInc
        MS_ColInc --> MS_ColCond
        
        MS_ColCond -->|Нет| MS_RowLoop[startRow = 1]
        MS_RowLoop --> MS_RowCond{startRow < n?}
        MS_RowCond -->|Да| MS_Sum2[sum = 0<br/>i = startRow<br/>j = 0]
        MS_Sum2 --> MS_While2{<b>пока</b> i < n И j < n?}
        MS_While2 -->|Да| MS_Add2[sum += matrix[i][j]<br/>i++<br/>j++]
        MS_Add2 --> MS_While2
        MS_While2 -->|Нет| MS_Compare2{sum < minSum?}
        MS_Compare2 -->|Да| MS_Update2[minSum = sum]
        MS_Update2 --> MS_RowInc[startRow++]
        MS_Compare2 -->|Нет| MS_RowInc
        MS_RowInc --> MS_RowCond
        MS_RowCond -->|Нет| MS_Print[/Вывод: Минимум сумм диагоналей: minSum/]
        MS_Print --> MS_End([Конец])
    end

    %% Подробное описание deleteMatrix
    subgraph deleteMatrix_detail [deleteMatrix]
        DM_Start([Начало]) --> DM_i{i = 0}
        DM_i --> DM_iCond{i < n?}
        DM_iCond -->|Да| DM_Delete[delete[] matrix[i]]
        DM_Delete --> DM_iInc[i++]
        DM_iInc --> DM_iCond
        DM_iCond -->|Нет| DM_DeleteMat[delete[] matrix]
        DM_DeleteMat --> DM_End([Конец])
    end
