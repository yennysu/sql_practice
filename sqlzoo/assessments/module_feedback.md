# Module Feedback

### Source:
### https://sqlzoo.net/wiki/Module_Feedback

### 1. Find the name of the student with number 50200100

```sql
SELECT SPR_FNM1, SPR_SURN
FROM INS_SPR
WHERE SPR_CODE=50200100;
```

### 2. Show the module code and module name for modules studied by the student with number 50200100 in session 2016/7 TR1

```sql
SELECT a.MOD_CODE, b.MOD_NAME
FROM CAM_SMO a
JOIN INS_MOD b
    ON a.MOD_CODE = b.MOD_CODE
WHERE a.AYR_CODE='2016/7'
    AND a.PSL_CODE='TR1'
    AND a.SPR_CODE=50200100;
```

### 3. Show the module code and module name and details of the module leader for modules studied by the student with number 50200100 in session 2016/7 TR1

```sql
```

### 4. Show the Percentage of students who gave 4 or 5 to module SET08108 in session 2016/7 TR1

(note that this is not real data, these responses were randomly generated)

``` sql
```

### 5. For each response 1-5 show the number of students who gave that response (Module SET08108, 2016/7, TR1)

(note that this is not real data, these responses were randomly generated)

```sql
```