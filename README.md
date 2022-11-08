# DSAC-Hadoop-project
I have an created mapreduce project in Hadoop 


## Matrix-Matrix Multiplication



#### Workflow - 2 MapReduce jobs
1. CellMultiplication: Mapper1 + Mapper2 ---> Reducer  
    - Mapper1: read the non-zero elements from matrix1 file (i1 j1 *M1*[i1][j1])  
        input: <offset, line>  
        output: <j1, i1=*M1*[i1][j1]>
    - Mapper2: read the non-zero elements from matrix2 file (i2 j2 *M2*[i2][j2])  
        input: <offset, line>  
        output: <i2, j2:*M2*[i2][j2]>
    - Reducer: multiply a matrix1 cell with the corresponding matrix2 cell  
        input: <k, (i1=*M1*[i1][k], i2=*M1*[i2][k], ..., j1:*M2*[k][j1], j2:*M2*[k][j2], ...)>  
        output: <i:j, *M1*[i][k]**M2*[k][j]>

2. CellSum: Mapper ---> Reducer
    - Mapper: read the intermediate results of cell multiplication 
        input: <offset, line>  
        output: <i:j, *M1*[i][k]**M2*[k][j]> 
    
    - Reducer: sum up all the cell product to the final value for each vector row  
        input: <i:j, (*M1*[i][k1]**M2*[k1][j], *M1*[i][k2]**M2*[k2][j], ...)>  
        output: <i \t j, *M1*[i][k1]**M2*[k1][j] + *M1*[i][k2]**M2*[k2][j] + ...>
