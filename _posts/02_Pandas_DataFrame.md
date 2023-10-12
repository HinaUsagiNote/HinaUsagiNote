<b style='font-size:2em'>DataFrame(데이터프레임)</b>

# DataFrame 개요
- **표(table-행렬)** 를 다루는 Pandas의 타입.
    - Database의 Table이나 Excel의 표와 동일한 역할을 한다.
- 분석할 데이터를 가지는 판다스의 가장 핵심적인 클래스이다.
- **행(row)와 열(column)** 으로 구성되 있다.
- 각 행과 각 열은 식별자를 가지며 Series와 같이 두가지 종류가 있다.
    - **순번**
        - 양수, 음수 index 두가지를 가진다.
        - 컬럼도 내부적으로는 순번으로 관리되지만 우리가 조회할 때 사용할 수는 없다.
    - **이름**
        - 명시적으로 지정한 행과 열의 이름을 말한다.
        - 행의 이름은 **index name** 이라고 하고 열의 이름은 **column name**이라고 한다.
        - index name과 column name은 **중복될 수 있다.**
        - 명시적으로 지정하지 않으면 양수 순번이 index, column 이름으로 설정된다.
- 하나의 행과 하나의 열은 Series로 구성된다.
- DataFrame 객체는 직접 데이터를 넣어 생성하거나 데이터 셋을 파일(csv, 엑셀, DB 등)로 부터 읽어와 생성한다.

# DataFrame 생성
##  직접 생성
- `pd.DataFrame(data [, index=None, columns=None])`
    - data 
        - DataFrame을 구성할 값을 설정
            - Series, List, ndarray를 담은 2차원 배열
            - 열이름을 key로 컬럼의 값 value로 하는 딕션어리(사전)
    - index
        - index명으로 사용할 값 배열로 설정
    - columns
        - 컬럼명으로 사용할 값 배열로 설정


```python
import pandas as pd

# dictionary 이용
data = {
    "id":['id-1', 'id-2', 'id-3', 'id-4', 'id-5'], 
    "국어":[100, 80, 90, 70, 80],
    "영어":[80, 90, 75, 80, 60]
}
## key(컬럼명):value(1차원자료구조-컬럼의 값들)  - 각 value의 원소 개수는 동일
## index 이름: 따로 지정정하지 않으면(index=[index이름들]) 양수 index가 이름이 된다.
grade = pd.DataFrame(data)
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 리스트(튜플)을 이용해 생성
data2 = [
    [61, 72, 83], 
    [100, 90, 70],
    [100, 20, 50],
    [50, 100, 70]
]
## 2차원 구조의 리스트를 생성해서 DataFrame()에 전달
# grade2 = pd.DataFrame(data2)
grade2 = pd.DataFrame(data2, columns=["국어", "영어", "수학"],# column name 지정 
                             index=['id-1', 'id-2', 'id-3', 'id-4']) # index name 지정
grade2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>61</td>
      <td>72</td>
      <td>83</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>100</td>
      <td>90</td>
      <td>70</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>100</td>
      <td>20</td>
      <td>50</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>50</td>
      <td>100</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
type(grade2)
```




    pandas.core.frame.DataFrame



## DataFrame의 객체를 파일에 저장

- DataFrame객체는 다양한 형식의 파일로 저장할 수 있다.
- 기본구문
    - **`DataFrame객체.to_파일타입()`**

### CSV 파일로 저장
- `DataFrame객체.to_csv(파일경로,sep=',', index=True, header=True)`
    - 텍스트 파일로 저장
    - 파일경로: 저장할 파일경로(경로/파일명)
    - sep : 데이터 구분자
    - index, header: 인덱스/헤더 저장 여부
- encoding방식: UTF-8 로 저장된다.

- csv (comma separate value)
    - 표(table)을 text파일에 작성하는 형식
    - 한행에 한개의 데이터를 입력
    - 속성값들을 `,` 로 구분
```
10,20,30
100,10,5
1,40,70
```


```python
import os
# 저장파일 저장할 디렉토리 생성
os.makedirs('saved_data', exist_ok=True)
```


```python
# dataframe을 csv 파일로 저장. 파일명.csv
grade.to_csv("saved_data/grade.csv")
```


```python
grade.to_csv("saved_data/grade2.csv", index=False)  # index name은 저장하지 않기.(자동증가값일 경우 저장하지 않는다.)
```


```python
grade.to_csv("saved_data/grade3.csv", index=False, 
             header=False)  #컬럼명을 저장하지 않는다.
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>



### 엑셀로 저장
- `DataFrame객체.to_excel(파일경로, index=True, header=True)`


```python
# excel 파일로 저장/읽기 
## openpyxl 설치 (xlsx 형식), xlwt, xlrd (xls 형식)
```


```python
!pip install openpyxl
```

    Collecting openpyxl
      Downloading openpyxl-3.1.2-py2.py3-none-any.whl (249 kB)
         ---------------------------------------- 0.0/250.0 kB ? eta -:--:--
         - -------------------------------------- 10.2/250.0 kB ? eta -:--:--
         ------------ ------------------------ 81.9/250.0 kB 907.3 kB/s eta 0:00:01
         -------------------------------------  245.8/250.0 kB 2.1 MB/s eta 0:00:01
         -------------------------------------- 250.0/250.0 kB 1.9 MB/s eta 0:00:00
    Collecting et-xmlfile (from openpyxl)
      Downloading et_xmlfile-1.1.0-py3-none-any.whl (4.7 kB)
    Installing collected packages: et-xmlfile, openpyxl
    Successfully installed et-xmlfile-1.1.0 openpyxl-3.1.2
    


```python
# grade.to_excel("saved_data/grade.xlsx")
# grade.to_excel("saved_data/grade.xlsx", index=False)
grade.to_excel("saved_data/grade.xlsx", index=False, header=False)
```

### 기타 형식


```python
# pickle
grade.to_pickle('saved_data/grade.pkl')
```


```python
# HTML 파일 -> html 파일안에 <table> 태그로 구성.
grade.to_html('saved_data/grade.html', index=False)
```


## 파일로 부터 데이터셋을 읽어와 생성하기

### csv 파일 등 텍스트 파일로 부터 읽어와 생성
- `pd.read_csv(파일경로, sep=',', header, index_col, na_values)`
    - **파일경로** 
        - 읽어올 파일의 경로
    - **sep**=","
        - 데이터 구분자. 
        - 기본값: 쉼표
    - **header**=정수
        - 열이름(컬럼이름)으로 사용할 행 지정
        - 기본값: 첫번째 행
        - None을 설정하면 Header는 없다는 것으로 파일의 첫번째 행부터 값으로 사용하고 컬럼명은 0부터 자동증가하는 값을 붙인다.
    - **index_col**=정수,컬럼명
        - index 명으로 사용할 열이름(문자열)이나 열의 순번(정수)을 지정.
        - 생략시 0부터 자동증가하는 값을 붙인다.
    - **na_values**
        - 읽어올 데이터셋의 값 중 결측치로 처리할 문자열 지정. 


```python
# index이름, column이름 모두 저장된 csv 파일

## csv파일의 첫행 -> header(컬럼명), 
##   index이름은 따로 읽지 않음.(첫 열을 데이터로 읽는다.) 
###  index이름으로 양수 index를 사용.
# grade_r1 = pd.read_csv('saved_data/grade.csv')
grade_r1 = pd.read_csv('saved_data/grade.csv',
                       index_col=0) # 0번 컬럼을 index이름으로 사용.

grade_r1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>0</th>
      <th>id-1</th>
      <th>100</th>
      <th>80</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# header 는 있고 index name은 없는 csv
grade_r2 = pd.read_csv('saved_data/grade2.csv', 
                       index_col="id")  # id컬럼을 index 이름으로 사용.
grade_r2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# header가 없은 csv
# grade_r3 = pd.read_csv('saved_data/grade3.csv') # 첫행이 header가 됨.
grade_r3 = pd.read_csv('saved_data/grade3.csv', 
                       header=None,  #header가 없다.
                       names=['ID','국어', '영어'] # 컬럼명을 명시
                      )

grade_r3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 값의 구분자가 `,` 가 아닌경우(tab)
grade_r5 = pd.read_csv("saved_data/grade4.csv", sep="\t")

grade_r5
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade_r6 = pd.read_csv('saved_data/grade5.csv', na_values='없음')
# na_values=["없음", 'id-1'])

grade_r6
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>NaN</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80.0</td>
      <td>60.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade_r6.isnull().sum()
```




    id    0
    국어    1
    영어    2
    dtype: int64




```python
pd.read_csv
```

    Help on function read_csv in module pandas.io.parsers.readers:
    
    read_csv(filepath_or_buffer: 'FilePath | ReadCsvBuffer[bytes] | ReadCsvBuffer[str]', *, sep: 'str | None | lib.NoDefault' = <no_default>, delimiter: 'str | None | lib.NoDefault' = None, header: "int | Sequence[int] | None | Literal['infer']" = 'infer', names: 'Sequence[Hashable] | None | lib.NoDefault' = <no_default>, index_col: 'IndexLabel | Literal[False] | None' = None, usecols: 'list[HashableT] | Callable[[Hashable], bool] | None' = None, dtype: 'DtypeArg | None' = None, engine: 'CSVEngine | None' = None, converters: 'Mapping[Hashable, Callable] | None' = None, true_values: 'list | None' = None, false_values: 'list | None' = None, skipinitialspace: 'bool' = False, skiprows: 'list[int] | int | Callable[[Hashable], bool] | None' = None, skipfooter: 'int' = 0, nrows: 'int | None' = None, na_values: 'Sequence[str] | Mapping[str, Sequence[str]] | None' = None, keep_default_na: 'bool' = True, na_filter: 'bool' = True, verbose: 'bool' = False, skip_blank_lines: 'bool' = True, parse_dates: 'bool | Sequence[Hashable] | None' = None, infer_datetime_format: 'bool | lib.NoDefault' = <no_default>, keep_date_col: 'bool' = False, date_parser: 'Callable | lib.NoDefault' = <no_default>, date_format: 'str | None' = None, dayfirst: 'bool' = False, cache_dates: 'bool' = True, iterator: 'bool' = False, chunksize: 'int | None' = None, compression: 'CompressionOptions' = 'infer', thousands: 'str | None' = None, decimal: 'str' = '.', lineterminator: 'str | None' = None, quotechar: 'str' = '"', quoting: 'int' = 0, doublequote: 'bool' = True, escapechar: 'str | None' = None, comment: 'str | None' = None, encoding: 'str | None' = None, encoding_errors: 'str | None' = 'strict', dialect: 'str | csv.Dialect | None' = None, on_bad_lines: 'str' = 'error', delim_whitespace: 'bool' = False, low_memory: 'bool' = True, memory_map: 'bool' = False, float_precision: "Literal['high', 'legacy'] | None" = None, storage_options: 'StorageOptions | None' = None, dtype_backend: 'DtypeBackend | lib.NoDefault' = <no_default>) -> 'DataFrame | TextFileReader'
        Read a comma-separated values (csv) file into DataFrame.
        
        Also supports optionally iterating or breaking of the file
        into chunks.
        
        Additional help can be found in the online docs for
        `IO Tools <https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html>`_.
        
        Parameters
        ----------
        filepath_or_buffer : str, path object or file-like object
            Any valid string path is acceptable. The string could be a URL. Valid
            URL schemes include http, ftp, s3, gs, and file. For file URLs, a host is
            expected. A local file could be: file://localhost/path/to/table.csv.
        
            If you want to pass in a path object, pandas accepts any ``os.PathLike``.
        
            By file-like object, we refer to objects with a ``read()`` method, such as
            a file handle (e.g. via builtin ``open`` function) or ``StringIO``.
        sep : str, default ','
            Character or regex pattern to treat as the delimiter. If ``sep=None``, the
            C engine cannot automatically detect
            the separator, but the Python parsing engine can, meaning the latter will
            be used and automatically detect the separator from only the first valid
            row of the file by Python's builtin sniffer tool, ``csv.Sniffer``.
            In addition, separators longer than 1 character and different from
            ``'\s+'`` will be interpreted as regular expressions and will also force
            the use of the Python parsing engine. Note that regex delimiters are prone
            to ignoring quoted data. Regex example: ``'\r\t'``.
        delimiter : str, optional
            Alias for ``sep``.
        header : int, Sequence of int, 'infer' or None, default 'infer'
            Row number(s) containing column labels and marking the start of the
            data (zero-indexed). Default behavior is to infer the column names: if no ``names``
            are passed the behavior is identical to ``header=0`` and column
            names are inferred from the first line of the file, if column
            names are passed explicitly to ``names`` then the behavior is identical to
            ``header=None``. Explicitly pass ``header=0`` to be able to
            replace existing names. The header can be a list of integers that
            specify row locations for a :class:`~pandas.MultiIndex` on the columns
            e.g. ``[0, 1, 3]``. Intervening rows that are not specified will be
            skipped (e.g. 2 in this example is skipped). Note that this
            parameter ignores commented lines and empty lines if
            ``skip_blank_lines=True``, so ``header=0`` denotes the first line of
            data rather than the first line of the file.
        names : Sequence of Hashable, optional
            Sequence of column labels to apply. If the file contains a header row,
            then you should explicitly pass ``header=0`` to override the column names.
            Duplicates in this list are not allowed.
        index_col : Hashable, Sequence of Hashable or False, optional
          Column(s) to use as row label(s), denoted either by column labels or column
          indices.  If a sequence of labels or indices is given, :class:`~pandas.MultiIndex`
          will be formed for the row labels.
        
          Note: ``index_col=False`` can be used to force pandas to *not* use the first
          column as the index, e.g., when you have a malformed file with delimiters at
          the end of each line.
        usecols : list of Hashable or Callable, optional
            Subset of columns to select, denoted either by column labels or column indices.
            If list-like, all elements must either
            be positional (i.e. integer indices into the document columns) or strings
            that correspond to column names provided either by the user in ``names`` or
            inferred from the document header row(s). If ``names`` are given, the document
            header row(s) are not taken into account. For example, a valid list-like
            ``usecols`` parameter would be ``[0, 1, 2]`` or ``['foo', 'bar', 'baz']``.
            Element order is ignored, so ``usecols=[0, 1]`` is the same as ``[1, 0]``.
            To instantiate a :class:`~pandas.DataFrame` from ``data`` with element order
            preserved use ``pd.read_csv(data, usecols=['foo', 'bar'])[['foo', 'bar']]``
            for columns in ``['foo', 'bar']`` order or
            ``pd.read_csv(data, usecols=['foo', 'bar'])[['bar', 'foo']]``
            for ``['bar', 'foo']`` order.
        
            If callable, the callable function will be evaluated against the column
            names, returning names where the callable function evaluates to ``True``. An
            example of a valid callable argument would be ``lambda x: x.upper() in
            ['AAA', 'BBB', 'DDD']``. Using this parameter results in much faster
            parsing time and lower memory usage.
        dtype : dtype or dict of {Hashable : dtype}, optional
            Data type(s) to apply to either the whole dataset or individual columns.
            E.g., ``{'a': np.float64, 'b': np.int32, 'c': 'Int64'}``
            Use ``str`` or ``object`` together with suitable ``na_values`` settings
            to preserve and not interpret ``dtype``.
            If ``converters`` are specified, they will be applied INSTEAD
            of ``dtype`` conversion.
        
            .. versionadded:: 1.5.0
        
                Support for ``defaultdict`` was added. Specify a ``defaultdict`` as input where
                the default determines the ``dtype`` of the columns which are not explicitly
                listed.
        engine : {'c', 'python', 'pyarrow'}, optional
            Parser engine to use. The C and pyarrow engines are faster, while the python engine
            is currently more feature-complete. Multithreading is currently only supported by
            the pyarrow engine.
        
            .. versionadded:: 1.4.0
        
                The 'pyarrow' engine was added as an *experimental* engine, and some features
                are unsupported, or may not work correctly, with this engine.
        converters : dict of {Hashable : Callable}, optional
            Functions for converting values in specified columns. Keys can either
            be column labels or column indices.
        true_values : list, optional
            Values to consider as ``True`` in addition to case-insensitive variants of 'True'.
        false_values : list, optional
            Values to consider as ``False`` in addition to case-insensitive variants of 'False'.
        skipinitialspace : bool, default False
            Skip spaces after delimiter.
        skiprows : int, list of int or Callable, optional
            Line numbers to skip (0-indexed) or number of lines to skip (``int``)
            at the start of the file.
        
            If callable, the callable function will be evaluated against the row
            indices, returning ``True`` if the row should be skipped and ``False`` otherwise.
            An example of a valid callable argument would be ``lambda x: x in [0, 2]``.
        skipfooter : int, default 0
            Number of lines at bottom of file to skip (Unsupported with ``engine='c'``).
        nrows : int, optional
            Number of rows of file to read. Useful for reading pieces of large files.
        na_values : Hashable, Iterable of Hashable or dict of {Hashable : Iterable}, optional
            Additional strings to recognize as ``NA``/``NaN``. If ``dict`` passed, specific
            per-column ``NA`` values.  By default the following values are interpreted as
            ``NaN``: " ", "#N/A", "#N/A N/A", "#NA", "-1.#IND", "-1.#QNAN", "-NaN", "-nan",
            "1.#IND", "1.#QNAN", "<NA>", "N/A", "NA", "NULL", "NaN", "None",
            "n/a", "nan", "null ".
        
        keep_default_na : bool, default True
            Whether or not to include the default ``NaN`` values when parsing the data.
            Depending on whether ``na_values`` is passed in, the behavior is as follows:
        
            * If ``keep_default_na`` is ``True``, and ``na_values`` are specified, ``na_values``
              is appended to the default ``NaN`` values used for parsing.
            * If ``keep_default_na`` is ``True``, and ``na_values`` are not specified, only
              the default ``NaN`` values are used for parsing.
            * If ``keep_default_na`` is ``False``, and ``na_values`` are specified, only
              the ``NaN`` values specified ``na_values`` are used for parsing.
            * If ``keep_default_na`` is ``False``, and ``na_values`` are not specified, no
              strings will be parsed as ``NaN``.
        
            Note that if ``na_filter`` is passed in as ``False``, the ``keep_default_na`` and
            ``na_values`` parameters will be ignored.
        na_filter : bool, default True
            Detect missing value markers (empty strings and the value of ``na_values``). In
            data without any ``NA`` values, passing ``na_filter=False`` can improve the
            performance of reading a large file.
        verbose : bool, default False
            Indicate number of ``NA`` values placed in non-numeric columns.
        skip_blank_lines : bool, default True
            If ``True``, skip over blank lines rather than interpreting as ``NaN`` values.
        parse_dates : bool, list of Hashable, list of lists or dict of {Hashable : list}, default False
            The behavior is as follows:
        
            * ``bool``. If ``True`` -> try parsing the index.
            * ``list`` of ``int`` or names. e.g. If ``[1, 2, 3]`` -> try parsing columns 1, 2, 3
              each as a separate date column.
            * ``list`` of ``list``. e.g.  If ``[[1, 3]]`` -> combine columns 1 and 3 and parse
              as a single date column.
            * ``dict``, e.g. ``{'foo' : [1, 3]}`` -> parse columns 1, 3 as date and call
              result 'foo'
        
            If a column or index cannot be represented as an array of ``datetime``,
            say because of an unparsable value or a mixture of timezones, the column
            or index will be returned unaltered as an ``object`` data type. For
            non-standard ``datetime`` parsing, use :func:`~pandas.to_datetime` after
            :func:`~pandas.read_csv`.
        
            Note: A fast-path exists for iso8601-formatted dates.
        infer_datetime_format : bool, default False
            If ``True`` and ``parse_dates`` is enabled, pandas will attempt to infer the
            format of the ``datetime`` strings in the columns, and if it can be inferred,
            switch to a faster method of parsing them. In some cases this can increase
            the parsing speed by 5-10x.
        
            .. deprecated:: 2.0.0
                A strict version of this argument is now the default, passing it has no effect.
        
        keep_date_col : bool, default False
            If ``True`` and ``parse_dates`` specifies combining multiple columns then
            keep the original columns.
        date_parser : Callable, optional
            Function to use for converting a sequence of string columns to an array of
            ``datetime`` instances. The default uses ``dateutil.parser.parser`` to do the
            conversion. pandas will try to call ``date_parser`` in three different ways,
            advancing to the next if an exception occurs: 1) Pass one or more arrays
            (as defined by ``parse_dates``) as arguments; 2) concatenate (row-wise) the
            string values from the columns defined by ``parse_dates`` into a single array
            and pass that; and 3) call ``date_parser`` once for each row using one or
            more strings (corresponding to the columns defined by ``parse_dates``) as
            arguments.
        
            .. deprecated:: 2.0.0
               Use ``date_format`` instead, or read in as ``object`` and then apply
               :func:`~pandas.to_datetime` as-needed.
        date_format : str or dict of column -> format, optional
           Format to use for parsing dates when used in conjunction with ``parse_dates``.
           For anything more complex, please read in as ``object`` and then apply
           :func:`~pandas.to_datetime` as-needed.
        
           .. versionadded:: 2.0.0
        dayfirst : bool, default False
            DD/MM format dates, international and European format.
        cache_dates : bool, default True
            If ``True``, use a cache of unique, converted dates to apply the ``datetime``
            conversion. May produce significant speed-up when parsing duplicate
            date strings, especially ones with timezone offsets.
        
        iterator : bool, default False
            Return ``TextFileReader`` object for iteration or getting chunks with
            ``get_chunk()``.
        
            .. versionchanged:: 1.2
        
               ``TextFileReader`` is a context manager.
        chunksize : int, optional
            Number of lines to read from the file per chunk. Passing a value will cause the
            function to return a ``TextFileReader`` object for iteration.
            See the `IO Tools docs
            <https://pandas.pydata.org/pandas-docs/stable/io.html#io-chunking>`_
            for more information on ``iterator`` and ``chunksize``.
        
            .. versionchanged:: 1.2
        
               ``TextFileReader`` is a context manager.
        compression : str or dict, default 'infer'
            For on-the-fly decompression of on-disk data. If 'infer' and 'filepath_or_buffer' is
            path-like, then detect compression from the following extensions: '.gz',
            '.bz2', '.zip', '.xz', '.zst', '.tar', '.tar.gz', '.tar.xz' or '.tar.bz2'
            (otherwise no compression).
            If using 'zip' or 'tar', the ZIP file must contain only one data file to be read in.
            Set to ``None`` for no decompression.
            Can also be a dict with key ``'method'`` set
            to one of {``'zip'``, ``'gzip'``, ``'bz2'``, ``'zstd'``, ``'xz'``, ``'tar'``} and
            other key-value pairs are forwarded to
            ``zipfile.ZipFile``, ``gzip.GzipFile``,
            ``bz2.BZ2File``, ``zstandard.ZstdDecompressor``, ``lzma.LZMAFile`` or
            ``tarfile.TarFile``, respectively.
            As an example, the following could be passed for Zstandard decompression using a
            custom compression dictionary:
            ``compression={'method': 'zstd', 'dict_data': my_compression_dict}``.
        
            .. versionadded:: 1.5.0
                Added support for `.tar` files.
        
            .. versionchanged:: 1.4.0 Zstandard support.
        
        thousands : str (length 1), optional
            Character acting as the thousands separator in numerical values.
        decimal : str (length 1), default '.'
            Character to recognize as decimal point (e.g., use ',' for European data).
        lineterminator : str (length 1), optional
            Character used to denote a line break. Only valid with C parser.
        quotechar : str (length 1), optional
            Character used to denote the start and end of a quoted item. Quoted
            items can include the ``delimiter`` and it will be ignored.
        quoting : {0 or csv.QUOTE_MINIMAL, 1 or csv.QUOTE_ALL, 2 or csv.QUOTE_NONNUMERIC, 3 or csv.QUOTE_NONE}, default csv.QUOTE_MINIMAL
            Control field quoting behavior per ``csv.QUOTE_*`` constants. Default is
            ``csv.QUOTE_MINIMAL`` (i.e., 0) which implies that only fields containing special
            characters are quoted (e.g., characters defined in ``quotechar``, ``delimiter``,
            or ``lineterminator``.
        doublequote : bool, default True
           When ``quotechar`` is specified and ``quoting`` is not ``QUOTE_NONE``, indicate
           whether or not to interpret two consecutive ``quotechar`` elements INSIDE a
           field as a single ``quotechar`` element.
        escapechar : str (length 1), optional
            Character used to escape other characters.
        comment : str (length 1), optional
            Character indicating that the remainder of line should not be parsed.
            If found at the beginning
            of a line, the line will be ignored altogether. This parameter must be a
            single character. Like empty lines (as long as ``skip_blank_lines=True``),
            fully commented lines are ignored by the parameter ``header`` but not by
            ``skiprows``. For example, if ``comment='#'``, parsing
            ``#empty\na,b,c\n1,2,3`` with ``header=0`` will result in ``'a,b,c'`` being
            treated as the header.
        encoding : str, optional, default 'utf-8'
            Encoding to use for UTF when reading/writing (ex. ``'utf-8'``). `List of Python
            standard encodings
            <https://docs.python.org/3/library/codecs.html#standard-encodings>`_ .
        
            .. versionchanged:: 1.2
        
               When ``encoding`` is ``None``, ``errors='replace'`` is passed to
               ``open()``. Otherwise, ``errors='strict'`` is passed to ``open()``.
               This behavior was previously only the case for ``engine='python'``.
        
            .. versionchanged:: 1.3.0
        
               ``encoding_errors`` is a new argument. ``encoding`` has no longer an
               influence on how encoding errors are handled.
        
        encoding_errors : str, optional, default 'strict'
            How encoding errors are treated. `List of possible values
            <https://docs.python.org/3/library/codecs.html#error-handlers>`_ .
        
            .. versionadded:: 1.3.0
        
        dialect : str or csv.Dialect, optional
            If provided, this parameter will override values (default or not) for the
            following parameters: ``delimiter``, ``doublequote``, ``escapechar``,
            ``skipinitialspace``, ``quotechar``, and ``quoting``. If it is necessary to
            override values, a ``ParserWarning`` will be issued. See ``csv.Dialect``
            documentation for more details.
        on_bad_lines : {'error', 'warn', 'skip'} or Callable, default 'error'
            Specifies what to do upon encountering a bad line (a line with too many fields).
            Allowed values are :
        
            - ``'error'``, raise an Exception when a bad line is encountered.
            - ``'warn'``, raise a warning when a bad line is encountered and skip that line.
            - ``'skip'``, skip bad lines without raising or warning when they are encountered.
        
            .. versionadded:: 1.3.0
        
            .. versionadded:: 1.4.0
        
                - Callable, function with signature
                  ``(bad_line: list[str]) -> list[str] | None`` that will process a single
                  bad line. ``bad_line`` is a list of strings split by the ``sep``.
                  If the function returns ``None``, the bad line will be ignored.
                  If the function returns a new ``list`` of strings with more elements than
                  expected, a ``ParserWarning`` will be emitted while dropping extra elements.
                  Only supported when ``engine='python'``
        
        delim_whitespace : bool, default False
            Specifies whether or not whitespace (e.g. ``' '`` or ``'\t'``) will be
            used as the ``sep`` delimiter. Equivalent to setting ``sep='\s+'``. If this option
            is set to ``True``, nothing should be passed in for the ``delimiter``
            parameter.
        low_memory : bool, default True
            Internally process the file in chunks, resulting in lower memory use
            while parsing, but possibly mixed type inference.  To ensure no mixed
            types either set ``False``, or specify the type with the ``dtype`` parameter.
            Note that the entire file is read into a single :class:`~pandas.DataFrame`
            regardless, use the ``chunksize`` or ``iterator`` parameter to return the data in
            chunks. (Only valid with C parser).
        memory_map : bool, default False
            If a filepath is provided for ``filepath_or_buffer``, map the file object
            directly onto memory and access the data directly from there. Using this
            option can improve performance because there is no longer any I/O overhead.
        float_precision : {'high', 'legacy', 'round_trip'}, optional
            Specifies which converter the C engine should use for floating-point
            values. The options are ``None`` or ``'high'`` for the ordinary converter,
            ``'legacy'`` for the original lower precision pandas converter, and
            ``'round_trip'`` for the round-trip converter.
        
            .. versionchanged:: 1.2
        
        storage_options : dict, optional
            Extra options that make sense for a particular storage connection, e.g.
            host, port, username, password, etc. For HTTP(S) URLs the key-value pairs
            are forwarded to ``urllib.request.Request`` as header options. For other
            URLs (e.g. starting with "s3://", and "gcs://") the key-value pairs are
            forwarded to ``fsspec.open``. Please see ``fsspec`` and ``urllib`` for more
            details, and for more examples on storage options refer `here
            <https://pandas.pydata.org/docs/user_guide/io.html?
            highlight=storage_options#reading-writing-remote-files>`_.
        
            .. versionadded:: 1.2
        
        dtype_backend : {'numpy_nullable', 'pyarrow'}, default 'numpy_nullable'
            Back-end data type applied to the resultant :class:`DataFrame`
            (still experimental). Behaviour is as follows:
        
            * ``"numpy_nullable"``: returns nullable-dtype-backed :class:`DataFrame`
              (default).
            * ``"pyarrow"``: returns pyarrow-backed nullable :class:`ArrowDtype`
              DataFrame.
        
            .. versionadded:: 2.0
        
        Returns
        -------
        DataFrame or TextFileReader
            A comma-separated values (csv) file is returned as two-dimensional
            data structure with labeled axes.
        
        See Also
        --------
        DataFrame.to_csv : Write DataFrame to a comma-separated values (csv) file.
        read_table : Read general delimited file into DataFrame.
        read_fwf : Read a table of fixed-width formatted lines into DataFrame.
        
        Examples
        --------
        >>> pd.read_csv('data.csv')  # doctest: +SKIP
    
    None
    


```python
##### header가 두줄 (두줄이상인 경우)
grade_r7 = pd.read_csv('saved_data/grade6.csv', header=[0, 1])

grade_r7
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="3" halign="left">1반</th>
    </tr>
    <tr>
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>



### 엑셀파일 읽기


```python
grade_xl = pd.read_excel('saved_data/grade.xlsx', header=None,
                         names=["id", "국어", "영어"])
grade_xl
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>



### 기타


```python
grade_pkl = pd.read_pickle("saved_data/grade.pkl")
grade_pkl
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# read_html() -> lxml 라이브러리 설치
!pip install lxml
```

    Collecting lxml
      Obtaining dependency information for lxml from https://files.pythonhosted.org/packages/31/58/e3b3dd6bb2ab7404f1f4992e2d0e6926ed40cef8ce1b3bbefd95877499e1/lxml-4.9.3-cp311-cp311-win_amd64.whl.metadata
      Downloading lxml-4.9.3-cp311-cp311-win_amd64.whl.metadata (3.9 kB)
    Downloading lxml-4.9.3-cp311-cp311-win_amd64.whl (3.8 MB)
       ---------------------------------------- 0.0/3.8 MB ? eta -:--:--
       ---------------------------------------- 0.0/3.8 MB ? eta -:--:--
        --------------------------------------- 0.1/3.8 MB 1.7 MB/s eta 0:00:03
       ------------------------------ --------- 2.9/3.8 MB 30.7 MB/s eta 0:00:01
       ---------------------------------------- 3.8/3.8 MB 34.5 MB/s eta 0:00:00
    Installing collected packages: lxml
    Successfully installed lxml-4.9.3
    


```python
grade_html = pd.read_html("saved_data/grade.html")
print(type(grade_html))
print(len(grade_html))
# html 문서안에 테이블들을 DataFrame을 읽은뒤 리스트로 묶어서 반환.
```

    <class 'list'>
    1
    


```python
grade_html[0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>êµ­ì´</th>
      <th>ìì´</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
result = pd.read_html("https://ko.wikipedia.org/wiki/FIFA_%EC%9B%94%EB%93%9C%EC%BB%B5")
len(result)
```




    13




```python
result[0]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIFA 월드컵</th>
      <th>FIFA 월드컵.1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>종목</td>
      <td>축구</td>
    </tr>
    <tr>
      <th>2</th>
      <td>국가</td>
      <td>FIFA 가맹국들</td>
    </tr>
    <tr>
      <th>3</th>
      <td>주최 기관</td>
      <td>FIFA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>선수단</td>
      <td>203개국 (2014년 예선) 48개국 (본선)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>역사</td>
      <td>역사</td>
    </tr>
    <tr>
      <th>6</th>
      <td>설립</td>
      <td>1930년</td>
    </tr>
    <tr>
      <th>7</th>
      <td>최다 우승</td>
      <td>브라질 (5회)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>최근 우승</td>
      <td>아르헨티나 (2022년)</td>
    </tr>
  </tbody>
</table>
</div>




```python
result[1]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>대륙별 지역</th>
      <th>출전국 수</th>
      <th>본선 진출국 수</th>
      <th>본선 진출율</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>아프리카(CAF)</td>
      <td>55</td>
      <td>5</td>
      <td>9%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>아시아(AFC)</td>
      <td>46</td>
      <td>4.5 1</td>
      <td>9.7%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>오세아니아(OFC)</td>
      <td>16</td>
      <td>0.5 1</td>
      <td>3%</td>
    </tr>
    <tr>
      <th>3</th>
      <td>유럽(UEFA)</td>
      <td>53</td>
      <td>13</td>
      <td>24%</td>
    </tr>
    <tr>
      <th>4</th>
      <td>북중미카리브(CONCACAF)</td>
      <td>40</td>
      <td>3.5 1</td>
      <td>8.7%</td>
    </tr>
    <tr>
      <th>5</th>
      <td>남아메리카(CONMEBOL)</td>
      <td>10</td>
      <td>4.5 1</td>
      <td>45%</td>
    </tr>
    <tr>
      <th>6</th>
      <td>개최국(대륙을 불문하고 선정됨.)</td>
      <td>1</td>
      <td>1</td>
      <td>100%</td>
    </tr>
    <tr>
      <th>7</th>
      <td>총합</td>
      <td>220</td>
      <td>32</td>
      <td>14.5%</td>
    </tr>
  </tbody>
</table>
</div>




```python
## DBMS에서 table을 조회(select)한 결과를 DataFrame에 담기.
## read_sql("select문", connection)
```


```python
# UserWarning이 출력이 안되도록 처리.
import warnings  
warnings.filterwarnings("ignore")
```


```python
import pymysql 
import pandas as pd

conn = pymysql.connect(host="localhost", port=3306, 
                       user='AAAA', password='BBBB', db='hr_join')
emp_df = pd.read_sql("select * from emp", conn)
dept_df = pd.read_sql("select dept_name, loc from dept", conn)
join_sql = "select e.emp_id, e.emp_name, d.dept_name \
            from emp e left join dept d on e.dept_id = d.dept_id \
            where e.salary > 5000"
result_df = pd.read_sql(join_sql, conn)
```


```python
result_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>emp_id</th>
      <th>emp_name</th>
      <th>dept_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100</td>
      <td>Steven</td>
      <td>Executive</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>Neena</td>
      <td>Executive</td>
    </tr>
    <tr>
      <th>2</th>
      <td>102</td>
      <td>Lex</td>
      <td>Executive</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Alexander</td>
      <td>IT</td>
    </tr>
    <tr>
      <th>4</th>
      <td>104</td>
      <td>Bruce</td>
      <td>IT</td>
    </tr>
    <tr>
      <th>5</th>
      <td>108</td>
      <td>Nancy</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>6</th>
      <td>109</td>
      <td>Daniel</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>7</th>
      <td>110</td>
      <td>John</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>8</th>
      <td>111</td>
      <td>Ismael</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>9</th>
      <td>112</td>
      <td>Jose Manuel</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>10</th>
      <td>113</td>
      <td>Luis</td>
      <td>Finance</td>
    </tr>
    <tr>
      <th>11</th>
      <td>114</td>
      <td>Den</td>
      <td>Purchasing</td>
    </tr>
    <tr>
      <th>12</th>
      <td>115</td>
      <td>Alexander</td>
      <td>Purchasing</td>
    </tr>
    <tr>
      <th>13</th>
      <td>120</td>
      <td>Matthew</td>
      <td>Shipping</td>
    </tr>
    <tr>
      <th>14</th>
      <td>121</td>
      <td>Adam</td>
      <td>Shipping</td>
    </tr>
    <tr>
      <th>15</th>
      <td>122</td>
      <td>Payam</td>
      <td>Shipping</td>
    </tr>
    <tr>
      <th>16</th>
      <td>123</td>
      <td>Shanta</td>
      <td>Shipping</td>
    </tr>
    <tr>
      <th>17</th>
      <td>124</td>
      <td>Kevin</td>
      <td>Shipping</td>
    </tr>
    <tr>
      <th>18</th>
      <td>145</td>
      <td>John</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>19</th>
      <td>146</td>
      <td>Karen</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>20</th>
      <td>147</td>
      <td>Alberto</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>21</th>
      <td>148</td>
      <td>Gerald</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>22</th>
      <td>149</td>
      <td>Eleni</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>23</th>
      <td>150</td>
      <td>Peter</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>24</th>
      <td>151</td>
      <td>David</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>25</th>
      <td>152</td>
      <td>Peter</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>26</th>
      <td>153</td>
      <td>Christopher</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>27</th>
      <td>154</td>
      <td>Nanette</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>28</th>
      <td>155</td>
      <td>Oliver</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>29</th>
      <td>156</td>
      <td>Janette</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>30</th>
      <td>157</td>
      <td>Patrick</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>31</th>
      <td>158</td>
      <td>Allan</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>32</th>
      <td>159</td>
      <td>Lindsey</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>33</th>
      <td>160</td>
      <td>Louise</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>34</th>
      <td>161</td>
      <td>Sarath</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>35</th>
      <td>162</td>
      <td>Clara</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>36</th>
      <td>163</td>
      <td>Danielle</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>37</th>
      <td>164</td>
      <td>Mattea</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>38</th>
      <td>165</td>
      <td>David</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>39</th>
      <td>166</td>
      <td>Sundar</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>40</th>
      <td>167</td>
      <td>Amit</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>41</th>
      <td>168</td>
      <td>Lisa</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>42</th>
      <td>169</td>
      <td>Harrison</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>43</th>
      <td>170</td>
      <td>Tayler</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>44</th>
      <td>171</td>
      <td>William</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>45</th>
      <td>172</td>
      <td>Elizabeth</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>46</th>
      <td>173</td>
      <td>Sundita</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>47</th>
      <td>174</td>
      <td>Ellen</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>48</th>
      <td>175</td>
      <td>Alyssa</td>
      <td>None</td>
    </tr>
    <tr>
      <th>49</th>
      <td>176</td>
      <td>Jonathon</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>50</th>
      <td>177</td>
      <td>Jack</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>51</th>
      <td>178</td>
      <td>Kimberely</td>
      <td>None</td>
    </tr>
    <tr>
      <th>52</th>
      <td>179</td>
      <td>Charles</td>
      <td>Sales</td>
    </tr>
    <tr>
      <th>53</th>
      <td>201</td>
      <td>Michael</td>
      <td>Marketing</td>
    </tr>
    <tr>
      <th>54</th>
      <td>202</td>
      <td>Pat</td>
      <td>Marketing</td>
    </tr>
    <tr>
      <th>55</th>
      <td>203</td>
      <td>Susan</td>
      <td>Human Resources</td>
    </tr>
    <tr>
      <th>56</th>
      <td>204</td>
      <td>Hermann</td>
      <td>Public Relations</td>
    </tr>
    <tr>
      <th>57</th>
      <td>205</td>
      <td>Shelley</td>
      <td>Accounting</td>
    </tr>
    <tr>
      <th>58</th>
      <td>206</td>
      <td>William</td>
      <td>Accounting</td>
    </tr>
  </tbody>
</table>
</div>




```python
emp_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>emp_id</th>
      <th>emp_name</th>
      <th>job_id</th>
      <th>mgr_id</th>
      <th>hire_date</th>
      <th>salary</th>
      <th>comm_pct</th>
      <th>dept_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100</td>
      <td>Steven</td>
      <td>AD_PRES</td>
      <td>NaN</td>
      <td>2003-06-17</td>
      <td>24000.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>101</td>
      <td>Neena</td>
      <td>AD_VP</td>
      <td>100.0</td>
      <td>2005-09-21</td>
      <td>17000.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>102</td>
      <td>Lex</td>
      <td>AD_VP</td>
      <td>100.0</td>
      <td>2001-01-13</td>
      <td>17000.0</td>
      <td>NaN</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>103</td>
      <td>Alexander</td>
      <td>IT_PROG</td>
      <td>102.0</td>
      <td>2006-01-03</td>
      <td>9000.0</td>
      <td>NaN</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>104</td>
      <td>Bruce</td>
      <td>IT_PROG</td>
      <td>103.0</td>
      <td>2007-05-21</td>
      <td>6000.0</td>
      <td>NaN</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>102</th>
      <td>202</td>
      <td>Pat</td>
      <td>MK_REP</td>
      <td>201.0</td>
      <td>2005-08-17</td>
      <td>6000.0</td>
      <td>NaN</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>103</th>
      <td>203</td>
      <td>Susan</td>
      <td>HR_REP</td>
      <td>101.0</td>
      <td>2002-06-07</td>
      <td>6500.0</td>
      <td>NaN</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>104</th>
      <td>204</td>
      <td>Hermann</td>
      <td>PR_REP</td>
      <td>101.0</td>
      <td>2002-06-07</td>
      <td>10000.0</td>
      <td>NaN</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>105</th>
      <td>205</td>
      <td>Shelley</td>
      <td>AC_MGR</td>
      <td>101.0</td>
      <td>2002-06-07</td>
      <td>12008.0</td>
      <td>NaN</td>
      <td>110.0</td>
    </tr>
    <tr>
      <th>106</th>
      <td>206</td>
      <td>William</td>
      <td>AC_ACCOUNT</td>
      <td>205.0</td>
      <td>2002-06-07</td>
      <td>8300.0</td>
      <td>NaN</td>
      <td>110.0</td>
    </tr>
  </tbody>
</table>
<p>107 rows × 8 columns</p>
</div>




```python
dept_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>dept_name</th>
      <th>loc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Administration</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marketing</td>
      <td>New York</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Purchasing</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Human Resources</td>
      <td>New York</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Shipping</td>
      <td>San Francisco</td>
    </tr>
    <tr>
      <th>5</th>
      <td>IT</td>
      <td>San Francisco</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Public Relations</td>
      <td>New York</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Sales</td>
      <td>New York</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Executive</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Finance</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Accounting</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Treasury</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Corporate Tax</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Control And Credit</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Shareholder Services</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Benefits</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Manufacturing</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Construction</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Contracting</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Operations</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>20</th>
      <td>IT Support</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>21</th>
      <td>NOC</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>22</th>
      <td>IT Helpdesk</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Government Sales</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Retail Sales</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Recruiting</td>
      <td>Seattle</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Payroll</td>
      <td>Seattle</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>


```python
#1. data/movie.csv 를 읽어서 변수 movie에 할당.
movie = pd.read_csv('data/movie.csv')
movie.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
#2. 1의 데이터프레임을 csv 파일로 저장(파일명 : saved_data/movie_df.csv)
movie.to_csv("saved_data/movie_df.csv", index=False)
```


```python
#3. 1의 데이터프레임을 엑셀파일로 저장(파일명 : saved_data/movie_df.xlsx)
movie.to_excel("saved_data/movie_df.xlsx", index=False)
```


```python
movie.to_pickle("saved_data/movie_df.pkl")
```


```python
#4. 3에서 저장한 movie_df.xlsx 파일을 읽어서 m_df 변수에 할당
m_df = pd.read_excel("saved_data/movie_df.xlsx")
m_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>




```python
m_df2 = pd.read_pickle('saved_data/movie_df.pkl')
m_df2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 28 columns</p>
</div>



# 주요 메소드, 속성 

![image.png](attachment:image.png)

![image.png](attachment:image.png)

## 데이터 프레임의 기본 정보 조회
- csv 파일 읽기
- shape
- info()
- head()
- tail()
- isnull().sum() 
    - 컬럼별 null 체크 (sum() 한번 더 하면 총개수)
- describe() : 숫자형-기술통계값, 범주형-총개수, 고유값들, 최빈값


```python
import pandas as pd

# 최대 출력 행수 (지정한 행이 넘어가면 앞뒤로 5개씩만 출력 - 기본: 60)
pd.options.display.max_rows = 100 
```


```python
# 최대 출력 컬럼수 (기본: 20)
pd.options.display.max_columns = 30  
```


```python
# 한 cell(컬럼)에서 보여줄 글자 최대수 (기본: 50)
pd.options.display.max_colwidth = 100
```


```python
# 파일로부터 읽기
df = pd.read_csv("data/movie.csv")
```


```python
# shape: 차원별 데이터 개수
df.shape  # (행수-값의 개수, 컬럼수-속성수)
```




    (4916, 28)




```python
# 일부 데이터를 확인 (앞에 N개, 뒤에 N개)
# 앞에 N개 - 기본 - 5개 행
df.head()
# df.head(10) # 앞에 10개
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>actor_3_name</th>
      <th>facenumber_in_poster</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>CCH Pounder</td>
      <td>Avatar</td>
      <td>886204</td>
      <td>4834</td>
      <td>Wes Studi</td>
      <td>0.0</td>
      <td>avatar|future|marine|native|paraplegic</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>Johnny Depp</td>
      <td>Pirates of the Caribbean: At World's End</td>
      <td>471220</td>
      <td>48350</td>
      <td>Jack Davenport</td>
      <td>0.0</td>
      <td>goddess|marriage ceremony|marriage proposal|pirate|singapore</td>
      <td>http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>Christoph Waltz</td>
      <td>Spectre</td>
      <td>275868</td>
      <td>11700</td>
      <td>Stephanie Sigman</td>
      <td>1.0</td>
      <td>bomb|espionage|sequel|spy|terrorist</td>
      <td>http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>Tom Hardy</td>
      <td>The Dark Knight Rises</td>
      <td>1144337</td>
      <td>106759</td>
      <td>Joseph Gordon-Levitt</td>
      <td>0.0</td>
      <td>deception|imprisonment|lawlessness|police officer|terrorist plot</td>
      <td>http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>Doug Walker</td>
      <td>Star Wars: Episode VII - The Force Awakens</td>
      <td>8</td>
      <td>143</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt5289954/?ref_=fn_tt_tt_1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Color</td>
      <td>Andrew Stanton</td>
      <td>462.0</td>
      <td>132.0</td>
      <td>475.0</td>
      <td>530.0</td>
      <td>Samantha Morton</td>
      <td>640.0</td>
      <td>73058679.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>Daryl Sabara</td>
      <td>John Carter</td>
      <td>212204</td>
      <td>1873</td>
      <td>Polly Walker</td>
      <td>1.0</td>
      <td>alien|american civil war|male nipple|mars|princess</td>
      <td>http://www.imdb.com/title/tt0401729/?ref_=fn_tt_tt_1</td>
      <td>738.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>263700000.0</td>
      <td>2012.0</td>
      <td>632.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>24000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Color</td>
      <td>Sam Raimi</td>
      <td>392.0</td>
      <td>156.0</td>
      <td>0.0</td>
      <td>4000.0</td>
      <td>James Franco</td>
      <td>24000.0</td>
      <td>336530303.0</td>
      <td>Action|Adventure|Romance</td>
      <td>J.K. Simmons</td>
      <td>Spider-Man 3</td>
      <td>383056</td>
      <td>46055</td>
      <td>Kirsten Dunst</td>
      <td>0.0</td>
      <td>sandman|spider man|symbiote|venom|villain</td>
      <td>http://www.imdb.com/title/tt0413300/?ref_=fn_tt_tt_1</td>
      <td>1902.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>258000000.0</td>
      <td>2007.0</td>
      <td>11000.0</td>
      <td>6.2</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Color</td>
      <td>Nathan Greno</td>
      <td>324.0</td>
      <td>100.0</td>
      <td>15.0</td>
      <td>284.0</td>
      <td>Donna Murphy</td>
      <td>799.0</td>
      <td>200807262.0</td>
      <td>Adventure|Animation|Comedy|Family|Fantasy|Musical|Romance</td>
      <td>Brad Garrett</td>
      <td>Tangled</td>
      <td>294810</td>
      <td>2036</td>
      <td>M.C. Gainey</td>
      <td>1.0</td>
      <td>17th century|based on fairy tale|disney|flower|tower</td>
      <td>http://www.imdb.com/title/tt0398286/?ref_=fn_tt_tt_1</td>
      <td>387.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>260000000.0</td>
      <td>2010.0</td>
      <td>553.0</td>
      <td>7.8</td>
      <td>1.85</td>
      <td>29000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Color</td>
      <td>Joss Whedon</td>
      <td>635.0</td>
      <td>141.0</td>
      <td>0.0</td>
      <td>19000.0</td>
      <td>Robert Downey Jr.</td>
      <td>26000.0</td>
      <td>458991599.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>Chris Hemsworth</td>
      <td>Avengers: Age of Ultron</td>
      <td>462669</td>
      <td>92000</td>
      <td>Scarlett Johansson</td>
      <td>4.0</td>
      <td>artificial intelligence|based on comic book|captain america|marvel cinematic universe|superhero</td>
      <td>http://www.imdb.com/title/tt2395427/?ref_=fn_tt_tt_1</td>
      <td>1117.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2015.0</td>
      <td>21000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>118000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Color</td>
      <td>David Yates</td>
      <td>375.0</td>
      <td>153.0</td>
      <td>282.0</td>
      <td>10000.0</td>
      <td>Daniel Radcliffe</td>
      <td>25000.0</td>
      <td>301956980.0</td>
      <td>Adventure|Family|Fantasy|Mystery</td>
      <td>Alan Rickman</td>
      <td>Harry Potter and the Half-Blood Prince</td>
      <td>321795</td>
      <td>58753</td>
      <td>Rupert Grint</td>
      <td>3.0</td>
      <td>blood|book|love|potion|professor</td>
      <td>http://www.imdb.com/title/tt0417741/?ref_=fn_tt_tt_1</td>
      <td>973.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG</td>
      <td>250000000.0</td>
      <td>2009.0</td>
      <td>11000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>10000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 뒤에 N개 - 기본 - 5개 행
df.tail() 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>actor_3_name</th>
      <th>facenumber_in_poster</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4913</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Maxwell Moody</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Drama|Horror|Thriller</td>
      <td>Eva Boehnke</td>
      <td>A Plague So Pleasant</td>
      <td>38</td>
      <td>0</td>
      <td>David Chandler</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2107644/?ref_=fn_tt_tt_1</td>
      <td>3.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>1400.0</td>
      <td>2013.0</td>
      <td>0.0</td>
      <td>6.3</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
      <td>Daniel Henney</td>
      <td>946.0</td>
      <td>10443.0</td>
      <td>Comedy|Drama|Romance</td>
      <td>Alan Ruck</td>
      <td>Shanghai Calling</td>
      <td>1255</td>
      <td>2386</td>
      <td>Eliza Coupe</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2070597/?ref_=fn_tt_tt_1</td>
      <td>9.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>NaN</td>
      <td>2012.0</td>
      <td>719.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>Brian Herzlinger</td>
      <td>86.0</td>
      <td>85222.0</td>
      <td>Documentary</td>
      <td>John August</td>
      <td>My Date with Drew</td>
      <td>4285</td>
      <td>163</td>
      <td>Jon Gunn</td>
      <td>0.0</td>
      <td>actress name in title|crush|date|four word title|video camera</td>
      <td>http://www.imdb.com/title/tt0378407/?ref_=fn_tt_tt_1</td>
      <td>84.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>1100.0</td>
      <td>2004.0</td>
      <td>23.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DataFrame에 대한 정보확인 (행, 컬럼)
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4916 entries, 0 to 4915
    Data columns (total 28 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   color                      4897 non-null   object 
     1   director_name              4814 non-null   object 
     2   num_critic_for_reviews     4867 non-null   float64
     3   duration                   4901 non-null   float64
     4   director_facebook_likes    4814 non-null   float64
     5   actor_3_facebook_likes     4893 non-null   float64
     6   actor_2_name               4903 non-null   object 
     7   actor_1_facebook_likes     4909 non-null   float64
     8   gross                      4054 non-null   float64
     9   genres                     4916 non-null   object 
     10  actor_1_name               4909 non-null   object 
     11  movie_title                4916 non-null   object 
     12  num_voted_users            4916 non-null   int64  
     13  cast_total_facebook_likes  4916 non-null   int64  
     14  actor_3_name               4893 non-null   object 
     15  facenumber_in_poster       4903 non-null   float64
     16  plot_keywords              4764 non-null   object 
     17  movie_imdb_link            4916 non-null   object 
     18  num_user_for_reviews       4895 non-null   float64
     19  language                   4902 non-null   object 
     20  country                    4911 non-null   object 
     21  content_rating             4616 non-null   object 
     22  budget                     4432 non-null   float64
     23  title_year                 4810 non-null   float64
     24  actor_2_facebook_likes     4903 non-null   float64
     25  imdb_score                 4916 non-null   float64
     26  aspect_ratio               4590 non-null   float64
     27  movie_facebook_likes       4916 non-null   int64  
    dtypes: float64(13), int64(3), object(12)
    memory usage: 1.1+ MB
    

```
RangeIndex: 4916(총행수) entries, 0 to 4915(index name)   #### 행정보
Data columns (total 28 columns):   ##### 컬럼정보
 #   Column(이름)                Non-Null Count(유효값개수)  Dtype(컬럼의 datatype)  
---  ------                     --------------              -----  
 0   color                      4897 non-null               object 
 1   director_name              4814 non-null               object 
 2   num_critic_for_reviews     4867 non-null               float64
 3   duration                   4901 non-null               float64
 4   director_facebook_likes    4814 non-null               float64
 5   actor_3_facebook_likes     4893 non-null               float64
 ...
 
dtypes: float64(13), int64(3), object(12)   ###### 데이터타입별 컬럼의 개수
memory usage: 1.1+ MB  ######## 차지하고 있는 메모리 크기.
```


```python
## 컬럼별 결측치 개수 확인
df.isnull().sum()   # dataframe.집계메소드() => 컬럼별로 계산 ===> 결과: Series
```




    color                         19
    director_name                102
    num_critic_for_reviews        49
    duration                      15
    director_facebook_likes      102
    actor_3_facebook_likes        23
    actor_2_name                  13
    actor_1_facebook_likes         7
    gross                        862
    genres                         0
    actor_1_name                   7
    movie_title                    0
    num_voted_users                0
    cast_total_facebook_likes      0
    actor_3_name                  23
    facenumber_in_poster          13
    plot_keywords                152
    movie_imdb_link                0
    num_user_for_reviews          21
    language                      14
    country                        5
    content_rating               300
    budget                       484
    title_year                   106
    actor_2_facebook_likes        13
    imdb_score                     0
    aspect_ratio                 326
    movie_facebook_likes           0
    dtype: int64




```python
df.isna().sum().sum()  # 총개수
```




    2656




```python
# 기술통계량을 조회
df.describe()  # 수치형(int, float) 컬럼들의 여러 통계량을 묶어서 계산.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>facenumber_in_poster</th>
      <th>num_user_for_reviews</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4867.000000</td>
      <td>4901.000000</td>
      <td>4814.000000</td>
      <td>4893.000000</td>
      <td>4909.000000</td>
      <td>4.054000e+03</td>
      <td>4.916000e+03</td>
      <td>4916.000000</td>
      <td>4903.000000</td>
      <td>4895.000000</td>
      <td>4.432000e+03</td>
      <td>4810.000000</td>
      <td>4903.000000</td>
      <td>4916.000000</td>
      <td>4590.000000</td>
      <td>4916.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>137.988905</td>
      <td>107.090798</td>
      <td>691.014541</td>
      <td>631.276313</td>
      <td>6494.488491</td>
      <td>4.764451e+07</td>
      <td>8.264492e+04</td>
      <td>9579.815907</td>
      <td>1.377320</td>
      <td>267.668846</td>
      <td>3.654749e+07</td>
      <td>2002.447609</td>
      <td>1621.923516</td>
      <td>6.437429</td>
      <td>2.222349</td>
      <td>7348.294142</td>
    </tr>
    <tr>
      <th>std</th>
      <td>120.239379</td>
      <td>25.286015</td>
      <td>2832.954125</td>
      <td>1625.874802</td>
      <td>15106.986884</td>
      <td>6.737255e+07</td>
      <td>1.383222e+05</td>
      <td>18164.316990</td>
      <td>2.023826</td>
      <td>372.934839</td>
      <td>1.002427e+08</td>
      <td>12.453977</td>
      <td>4011.299523</td>
      <td>1.127802</td>
      <td>1.402940</td>
      <td>19206.016458</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.620000e+02</td>
      <td>5.000000e+00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>2.180000e+02</td>
      <td>1916.000000</td>
      <td>0.000000</td>
      <td>1.600000</td>
      <td>1.180000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>49.000000</td>
      <td>93.000000</td>
      <td>7.000000</td>
      <td>132.000000</td>
      <td>607.000000</td>
      <td>5.019656e+06</td>
      <td>8.361750e+03</td>
      <td>1394.750000</td>
      <td>0.000000</td>
      <td>64.000000</td>
      <td>6.000000e+06</td>
      <td>1999.000000</td>
      <td>277.000000</td>
      <td>5.800000</td>
      <td>1.850000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>108.000000</td>
      <td>103.000000</td>
      <td>48.000000</td>
      <td>366.000000</td>
      <td>982.000000</td>
      <td>2.504396e+07</td>
      <td>3.313250e+04</td>
      <td>3049.000000</td>
      <td>1.000000</td>
      <td>153.000000</td>
      <td>1.985000e+07</td>
      <td>2005.000000</td>
      <td>593.000000</td>
      <td>6.600000</td>
      <td>2.350000</td>
      <td>159.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>191.000000</td>
      <td>118.000000</td>
      <td>189.750000</td>
      <td>633.000000</td>
      <td>11000.000000</td>
      <td>6.110841e+07</td>
      <td>9.377275e+04</td>
      <td>13616.750000</td>
      <td>2.000000</td>
      <td>320.500000</td>
      <td>4.300000e+07</td>
      <td>2011.000000</td>
      <td>912.000000</td>
      <td>7.200000</td>
      <td>2.350000</td>
      <td>2000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>813.000000</td>
      <td>511.000000</td>
      <td>23000.000000</td>
      <td>23000.000000</td>
      <td>640000.000000</td>
      <td>7.605058e+08</td>
      <td>1.689764e+06</td>
      <td>656730.000000</td>
      <td>43.000000</td>
      <td>5060.000000</td>
      <td>4.200000e+09</td>
      <td>2016.000000</td>
      <td>137000.000000</td>
      <td>9.500000</td>
      <td>16.000000</td>
      <td>349000.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe(include='object')  # 타입을 지정.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>actor_2_name</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>actor_3_name</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4897</td>
      <td>4814</td>
      <td>4903</td>
      <td>4916</td>
      <td>4909</td>
      <td>4916</td>
      <td>4893</td>
      <td>4764</td>
      <td>4916</td>
      <td>4902</td>
      <td>4911</td>
      <td>4616</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>2</td>
      <td>2397</td>
      <td>3030</td>
      <td>914</td>
      <td>2095</td>
      <td>4916</td>
      <td>3519</td>
      <td>4756</td>
      <td>4916</td>
      <td>46</td>
      <td>65</td>
      <td>18</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Color</td>
      <td>Steven Spielberg</td>
      <td>Morgan Freeman</td>
      <td>Drama</td>
      <td>Robert De Niro</td>
      <td>Avatar</td>
      <td>Steve Coogan</td>
      <td>based on novel</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>4693</td>
      <td>26</td>
      <td>18</td>
      <td>233</td>
      <td>48</td>
      <td>1</td>
      <td>8</td>
      <td>4</td>
      <td>1</td>
      <td>4582</td>
      <td>3710</td>
      <td>2067</td>
    </tr>
  </tbody>
</table>
</div>




```python
# include=[타입, 타입, ..]  지정한 타입의 컬럼에 대한 통계량을 계산
# exclude=[타입, ...]  지정한 타입 이외의 컬럼들에 대한 통계량 계산.
df.describe(include=['int32', 'int64'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>4.916000e+03</td>
      <td>4916.000000</td>
      <td>4916.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>8.264492e+04</td>
      <td>9579.815907</td>
      <td>7348.294142</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.383222e+05</td>
      <td>18164.316990</td>
      <td>19206.016458</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.000000e+00</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>8.361750e+03</td>
      <td>1394.750000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.313250e+04</td>
      <td>3049.000000</td>
      <td>159.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>9.377275e+04</td>
      <td>13616.750000</td>
      <td>2000.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.689764e+06</td>
      <td>656730.000000</td>
      <td>349000.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# dataframe.T  - 행과 열을 바꾼다. (전치-transpose)
df.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>num_critic_for_reviews</th>
      <td>4867.0</td>
      <td>1.379889e+02</td>
      <td>1.202394e+02</td>
      <td>1.00</td>
      <td>49.00</td>
      <td>108.00</td>
      <td>191.00</td>
      <td>8.130000e+02</td>
    </tr>
    <tr>
      <th>duration</th>
      <td>4901.0</td>
      <td>1.070908e+02</td>
      <td>2.528602e+01</td>
      <td>7.00</td>
      <td>93.00</td>
      <td>103.00</td>
      <td>118.00</td>
      <td>5.110000e+02</td>
    </tr>
    <tr>
      <th>director_facebook_likes</th>
      <td>4814.0</td>
      <td>6.910145e+02</td>
      <td>2.832954e+03</td>
      <td>0.00</td>
      <td>7.00</td>
      <td>48.00</td>
      <td>189.75</td>
      <td>2.300000e+04</td>
    </tr>
    <tr>
      <th>actor_3_facebook_likes</th>
      <td>4893.0</td>
      <td>6.312763e+02</td>
      <td>1.625875e+03</td>
      <td>0.00</td>
      <td>132.00</td>
      <td>366.00</td>
      <td>633.00</td>
      <td>2.300000e+04</td>
    </tr>
    <tr>
      <th>actor_1_facebook_likes</th>
      <td>4909.0</td>
      <td>6.494488e+03</td>
      <td>1.510699e+04</td>
      <td>0.00</td>
      <td>607.00</td>
      <td>982.00</td>
      <td>11000.00</td>
      <td>6.400000e+05</td>
    </tr>
    <tr>
      <th>gross</th>
      <td>4054.0</td>
      <td>4.764451e+07</td>
      <td>6.737255e+07</td>
      <td>162.00</td>
      <td>5019656.25</td>
      <td>25043962.00</td>
      <td>61108412.75</td>
      <td>7.605058e+08</td>
    </tr>
    <tr>
      <th>num_voted_users</th>
      <td>4916.0</td>
      <td>8.264492e+04</td>
      <td>1.383222e+05</td>
      <td>5.00</td>
      <td>8361.75</td>
      <td>33132.50</td>
      <td>93772.75</td>
      <td>1.689764e+06</td>
    </tr>
    <tr>
      <th>cast_total_facebook_likes</th>
      <td>4916.0</td>
      <td>9.579816e+03</td>
      <td>1.816432e+04</td>
      <td>0.00</td>
      <td>1394.75</td>
      <td>3049.00</td>
      <td>13616.75</td>
      <td>6.567300e+05</td>
    </tr>
    <tr>
      <th>facenumber_in_poster</th>
      <td>4903.0</td>
      <td>1.377320e+00</td>
      <td>2.023826e+00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.00</td>
      <td>2.00</td>
      <td>4.300000e+01</td>
    </tr>
    <tr>
      <th>num_user_for_reviews</th>
      <td>4895.0</td>
      <td>2.676688e+02</td>
      <td>3.729348e+02</td>
      <td>1.00</td>
      <td>64.00</td>
      <td>153.00</td>
      <td>320.50</td>
      <td>5.060000e+03</td>
    </tr>
    <tr>
      <th>budget</th>
      <td>4432.0</td>
      <td>3.654749e+07</td>
      <td>1.002427e+08</td>
      <td>218.00</td>
      <td>6000000.00</td>
      <td>19850000.00</td>
      <td>43000000.00</td>
      <td>4.200000e+09</td>
    </tr>
    <tr>
      <th>title_year</th>
      <td>4810.0</td>
      <td>2.002448e+03</td>
      <td>1.245398e+01</td>
      <td>1916.00</td>
      <td>1999.00</td>
      <td>2005.00</td>
      <td>2011.00</td>
      <td>2.016000e+03</td>
    </tr>
    <tr>
      <th>actor_2_facebook_likes</th>
      <td>4903.0</td>
      <td>1.621924e+03</td>
      <td>4.011300e+03</td>
      <td>0.00</td>
      <td>277.00</td>
      <td>593.00</td>
      <td>912.00</td>
      <td>1.370000e+05</td>
    </tr>
    <tr>
      <th>imdb_score</th>
      <td>4916.0</td>
      <td>6.437429e+00</td>
      <td>1.127802e+00</td>
      <td>1.60</td>
      <td>5.80</td>
      <td>6.60</td>
      <td>7.20</td>
      <td>9.500000e+00</td>
    </tr>
    <tr>
      <th>aspect_ratio</th>
      <td>4590.0</td>
      <td>2.222349e+00</td>
      <td>1.402940e+00</td>
      <td>1.18</td>
      <td>1.85</td>
      <td>2.35</td>
      <td>2.35</td>
      <td>1.600000e+01</td>
    </tr>
    <tr>
      <th>movie_facebook_likes</th>
      <td>4916.0</td>
      <td>7.348294e+03</td>
      <td>1.920602e+04</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>159.00</td>
      <td>2000.00</td>
      <td>3.490000e+05</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4916 entries, 0 to 4915
    Data columns (total 28 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   color                      4897 non-null   object 
     1   director_name              4814 non-null   object 
     2   num_critic_for_reviews     4867 non-null   float64
     3   duration                   4901 non-null   float64
     4   director_facebook_likes    4814 non-null   float64
     5   actor_3_facebook_likes     4893 non-null   float64
     6   actor_2_name               4903 non-null   object 
     7   actor_1_facebook_likes     4909 non-null   float64
     8   gross                      4054 non-null   float64
     9   genres                     4916 non-null   object 
     10  actor_1_name               4909 non-null   object 
     11  movie_title                4916 non-null   object 
     12  num_voted_users            4916 non-null   int64  
     13  cast_total_facebook_likes  4916 non-null   int64  
     14  actor_3_name               4893 non-null   object 
     15  facenumber_in_poster       4903 non-null   float64
     16  plot_keywords              4764 non-null   object 
     17  movie_imdb_link            4916 non-null   object 
     18  num_user_for_reviews       4895 non-null   float64
     19  language                   4902 non-null   object 
     20  country                    4911 non-null   object 
     21  content_rating             4616 non-null   object 
     22  budget                     4432 non-null   float64
     23  title_year                 4810 non-null   float64
     24  actor_2_facebook_likes     4903 non-null   float64
     25  imdb_score                 4916 non-null   float64
     26  aspect_ratio               4590 non-null   float64
     27  movie_facebook_likes       4916 non-null   int64  
    dtypes: float64(13), int64(3), object(12)
    memory usage: 1.1+ MB
    


```python
# 컬럼의 타입을 변경
df['num_voted_users'] = df['num_voted_users'].astype('int32')
```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4916 entries, 0 to 4915
    Data columns (total 28 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   color                      4897 non-null   object 
     1   director_name              4814 non-null   object 
     2   num_critic_for_reviews     4867 non-null   float64
     3   duration                   4901 non-null   float64
     4   director_facebook_likes    4814 non-null   float64
     5   actor_3_facebook_likes     4893 non-null   float64
     6   actor_2_name               4903 non-null   object 
     7   actor_1_facebook_likes     4909 non-null   float64
     8   gross                      4054 non-null   float64
     9   genres                     4916 non-null   object 
     10  actor_1_name               4909 non-null   object 
     11  movie_title                4916 non-null   object 
     12  num_voted_users            4916 non-null   int32  
     13  cast_total_facebook_likes  4916 non-null   int64  
     14  actor_3_name               4893 non-null   object 
     15  facenumber_in_poster       4903 non-null   float64
     16  plot_keywords              4764 non-null   object 
     17  movie_imdb_link            4916 non-null   object 
     18  num_user_for_reviews       4895 non-null   float64
     19  language                   4902 non-null   object 
     20  country                    4911 non-null   object 
     21  content_rating             4616 non-null   object 
     22  budget                     4432 non-null   float64
     23  title_year                 4810 non-null   float64
     24  actor_2_facebook_likes     4903 non-null   float64
     25  imdb_score                 4916 non-null   float64
     26  aspect_ratio               4590 non-null   float64
     27  movie_facebook_likes       4916 non-null   int64  
    dtypes: float64(13), int32(1), int64(2), object(12)
    memory usage: 1.0+ MB
    

# 컬럼이름/행이름 조회 및 변경
## 컬럼이름/행이름 조회
- **DataFrame객체.columns**
    - 컬럼명 조회
    - 컬럼명은 차후 조회를 위해 따로 변수에 저장하는 것이 좋다.
- **DataFrame객체.index**
    - 행명 조회


```python
import pandas as pd

grade = pd.read_csv('saved_data/grade2.csv')
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>국어</th>
      <th>영어</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼명들 조회 - Index객체로 반환 => 리스트구현체
grade.columns
```




    Index(['id', '국어', '영어'], dtype='object')




```python
# 자동증가 값(양수 index)를 사용한 경우: RangeIndex()
grade.index 
```




    RangeIndex(start=0, stop=5, step=1)




```python
grade2 = pd.read_csv('saved_data/grade2.csv', index_col=0)
grade2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# index이름이 명시적으로 설정된 경우: Index 객체
grade2.index  
```




    Index(['id-1', 'id-2', 'id-3', 'id-4', 'id-5'], dtype='object', name='id')



## 컬럼이름/행이름 변경
- columns와 index 속성으로는 통째로 바꾸는 것은 가능하나 일부만 선택해서 변경하는 것은 안된다.
    - `df.columns = ['새이름','새이름', ... , '새이름']`
    - `df.columns[1] = '새이름'` 
        - 이런식으로 개별적으로 변경은 안된다.


```python
# 통째로 바꿀때 사용.
grade.columns = ['ID', "KOREAN", "ENGLISH"]  
```


```python
# 개별변경은 안됨.
# grade.columns[1] = "KOR"  --- Error
```

### 컬럼이름/행이름 변경 관련 메소드    
- `DataFrame객체.rename(index=행이름변경설정, columns=열이름변경설정, inplace=False)`
    - **개별 컬럼이름/행이름 변경** 하는 메소드
    - 변경한 DataFrame을 반환
    - 변경설정: 딕셔너리 사용
        - {'기존이름':'새이름', ..}
        - inplace: 원본을 변경할지 여부(boolean)        


```python
# KOREAN -> 국어
grade.rename(columns={'KOREAN':'국어'}, inplace=True)
```


```python
grade.rename(columns={"국어":"KOR", "ENGLISH":"ENG"}, inplace=True)
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>KOR</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
#index 이름 변경
grade2.rename(index={"id-1":"abcde", 'id-3':'가나다'})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>abcde</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>가나다</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>



- `DataFrame객체.set_index(컬럼이름, inplace=False)`
    - 특정 컬럼을 행의 index 명으로 사용
    - 열이 index명이 되면서 그 컬럼은 Data Set 에서 제거된다.
- `DataFrame객체.reset_index(inplace=False)`
    - index를 첫번째 컬럼으로 복원


```python
result = grade.set_index("ID")
result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>KOR</th>
      <th>ENG</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
result2 = grade.set_index("KOR")
result2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>ENG</th>
    </tr>
    <tr>
      <th>KOR</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>id-1</td>
      <td>80</td>
    </tr>
    <tr>
      <th>80</th>
      <td>id-2</td>
      <td>90</td>
    </tr>
    <tr>
      <th>90</th>
      <td>id-3</td>
      <td>75</td>
    </tr>
    <tr>
      <th>70</th>
      <td>id-4</td>
      <td>80</td>
    </tr>
    <tr>
      <th>80</th>
      <td>id-5</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# index name을 컬럼으로 변경
result2.reset_index() 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>KOR</th>
      <th>ID</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100</td>
      <td>id-1</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>80</td>
      <td>id-2</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90</td>
      <td>id-3</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>70</td>
      <td>id-4</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80</td>
      <td>id-5</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# index name을 제거
result2.reset_index(drop=True)  
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# index 이름을 자동증가 값으로 변경하려고 할때 제거한다.
grade.iloc[[0, 2, 3]].reset_index(drop=True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>KOR</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
  </tbody>
</table>
</div>



# 행과 열의 값 변경

## 특정 행 또는 열 삭제
- DataFrame객체.drop(columns, index, inplace=False)
    - columns : 삭제할 열이름 또는 열이름 리스트
    - index : 삭제할 index명 또는 index 리스트
    - inplace: 원본을 변경할지 여부(boolean)


```python
# 행 삭제 -> index=index 이름 (순번-index 는 안됨.)
grade.drop(index=[1, 3])  
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>KOR</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id-1</td>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id-4</td>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id-5</td>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
#열 삭제
grade.drop(columns=['ID', 'ENG']) 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>KOR</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>80</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90</td>
    </tr>
    <tr>
      <th>3</th>
      <td>70</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80</td>
    </tr>
  </tbody>
</table>
</div>




```python
#axis: columns or 1,  axis: index or 0
## 0, 1 => axis(축) 번호 ===> 값이 나열된(차원)방향의 순번.
grade.drop(labels=["ID", "KOR"], axis=1) 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>75</td>
    </tr>
    <tr>
      <th>3</th>
      <td>80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
result = grade.drop(labels=[0, 3, 4], axis=0)
result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>KOR</th>
      <th>ENG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>id-2</td>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id-3</td>
      <td>90</td>
      <td>75</td>
    </tr>
  </tbody>
</table>
</div>




```python
## drop() 에서 행/컬럼의 이름 -> 이름으로만 삭제 가능. 순번으로는 삭제 안됨.
# ---------- Error
# grade.drop(columns=[0, 1])
# result.drop(index=[0])
```

## 열 추가
- 새로운 열을 지정 후 값을 대입하면 새로운 열을 추가할 수 있다.
    - 보통 **파생변수**를 만들 때 사용한다.
- **열 추가**
    - `df['새열명'] = 값`
    - 마지막 열로 추가된다.
    - 하나의 값을 대입하면 모든 행에 그 값이 대입된다.
    - 다른 값을 주려면 배열에 담아서 대입한다.
- **열 삽입**
    - `df.insert(삽입할 위치 index, 삽입할 열이름, 값)`    
- **파생변수생성**
    - 기존 열들의 값을 이용해서 만든 열을 파생변수라고 한다.
    - 벡터화 연산을 이용하여 값 대입한다.
    - df\['새열이름'\] = 기존 열들을 이용한 연산


```python
import pandas as pd

grade = pd.read_csv("saved_data/grade2.csv")
grade.set_index("id", inplace=True)
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼을 추가
## 컬럼값을 조회
grade["영어"]
```




    id
    id-1    80
    id-2    90
    id-3    75
    id-4    80
    id-5    60
    Name: 영어, dtype: int64




```python
## 컬럼을 추가
#수학컬럼을 생성. 모든행에 100을 대입. 없는 컬럼에 값 대입 => 추가
grade["수학"] = 100 
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>100</td>
    </tr>
  </tbody>
</table>
</div>




```python
#수학컬럼의 값을 변경. 있는 컬럼에 값을 대입 => 수정
grade["수학"] = 80   
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>80</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.Series([70, 100, 90, 40, 70], index=grade.index)
```




    id
    id-1     70
    id-2    100
    id-3     90
    id-4     40
    id-5     70
    dtype: int64




```python
# 값의 개수 = 행수. 1차원자료구조(리스트, 시리즈)
grade["수학"] = [70, 100, 90, 40, 70] 
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# series로 대입할 경우 index name이 같아야 한다.
grade['과학'] = pd.Series([70, 100, 90, 40, 70], index=grade.index)
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>과학</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>70</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>100</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼 조회 => Series 반환.
grade['국어'] 
```




    id
    id-1    100
    id-2     80
    id-3     90
    id-4     70
    id-5     80
    Name: 국어, dtype: int64




```python
grade['영어']
```




    id
    id-1    80
    id-2    90
    id-3    75
    id-4    80
    id-5    60
    Name: 영어, dtype: int64




```python
# series + serise
sum_result = grade['국어'] + grade['영어'] + grade['수학'] + grade['과학']
sum_result
```




    id
    id-1    320
    id-2    370
    id-3    345
    id-4    230
    id-5    280
    dtype: int64




```python
grade['총점'] = sum_result
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>과학</th>
      <th>총점</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>70</td>
      <td>320</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>100</td>
      <td>370</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>90</td>
      <td>345</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
      <td>40</td>
      <td>230</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
      <td>280</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade['평균'] = grade['총점'] // 4
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>과학</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>70</td>
      <td>320</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>100</td>
      <td>370</td>
      <td>92</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>90</td>
      <td>345</td>
      <td>86</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
      <td>40</td>
      <td>230</td>
      <td>57</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
      <td>280</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade.to_csv("saved_data/grade_result.csv")
```


```python
r = pd.read_csv("saved_data/grade_result.csv", index_col="id")

r
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>과학</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>70</td>
      <td>320</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>100</td>
      <td>370</td>
      <td>92</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>90</td>
      <td>345</td>
      <td>86</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
      <td>40</td>
      <td>230</td>
      <td>57</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
      <td>280</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade = pd.read_csv('saved_data/grade2.csv', index_col='id')
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
total = grade.sum(axis=1)
mean = grade.mean(axis=1)
grade['총점'] = total
grade['평균'] = mean
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>
- 패스 여부를 boolean값으로 저장하는 파생변수 열을 추가
    - 열이름: 합격여부
    - 평균점수가 80미만이면 False,이상이면 True가 나오도록 처리


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade['합격여부'] = grade['평균'] >= 80
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
import numpy as np

# True: 합격, False: 불합격   으로 변환
grade['통과여부2'] = np.where(grade['평균'] >= 80, "합격", "불합격") 
```


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>



# 행, 열의 값 조회
- indexer 연산자를 이용한다.
    - 행은 loc indexer, iloc indexer를 사용한다.
- 열은 indexing만 되고 slicing은 안된다.
- 행은 indexing, slicing 모두 지원한다.

## 열의 값 조회 
- **df['열이름']**
    - 열이름의 열 조회
- **df.열이름**
    - 열이름이 Python 식별자 규칙에 맞으면 **. 표기법**을 사용할 수 있다.
- **Fancy indexing**
    - 여러개의 열들을 한번에 조회할 경우 열이름들을 리스트로 묶어서 전달한다.
- **주의**
    - 열은 **순번으로는 조회할 수 없다.**
    - 열 조회 indexer에서 슬라이싱을 하면 **행 조회 Slicing이다.**
        - **만약 indexing이나 slicing을 이용해 열들을 조회하려면 columns 속성을 이용한다.**
            - `df[df.columns[:3]]`


```python
# 한개 열  조회 => Series 로 반환.
grade["국어"]  
```




    id
    id-1    100
    id-2     80
    id-3     90
    id-4     70
    id-5     80
    Name: 국어, dtype: int64




```python
grade.국어
```




    id
    id-1    100
    id-2     80
    id-3     90
    id-4     70
    id-5     80
    Name: 국어, dtype: int64




```python
# 여러 열을 조회 -> DataFrame 으로 반환.
grade[["평균", "총점"]]  
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>평균</th>
      <th>총점</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>90.0</td>
      <td>180</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>85.0</td>
      <td>170</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>82.5</td>
      <td>165</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>75.0</td>
      <td>150</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>70.0</td>
      <td>140</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼 순번으로는 조회안됨.
# grade[0] ---- Error
```


```python
# 컬럼조회는 slicing 할 수 없다.
```


```python
# 행 조회 
grade["국어":"평균"]  
grade[0:3]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade[grade.columns[:4]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>
- 다음 movie dataframe을 이용해 코드를 작성


```python
df = pd.read_csv("data/movie.csv")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>actor_3_name</th>
      <th>facenumber_in_poster</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>CCH Pounder</td>
      <td>Avatar</td>
      <td>886204</td>
      <td>4834</td>
      <td>Wes Studi</td>
      <td>0.0</td>
      <td>avatar|future|marine|native|paraplegic</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>Johnny Depp</td>
      <td>Pirates of the Caribbean: At World's End</td>
      <td>471220</td>
      <td>48350</td>
      <td>Jack Davenport</td>
      <td>0.0</td>
      <td>goddess|marriage ceremony|marriage proposal|pirate|singapore</td>
      <td>http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>Christoph Waltz</td>
      <td>Spectre</td>
      <td>275868</td>
      <td>11700</td>
      <td>Stephanie Sigman</td>
      <td>1.0</td>
      <td>bomb|espionage|sequel|spy|terrorist</td>
      <td>http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>Tom Hardy</td>
      <td>The Dark Knight Rises</td>
      <td>1144337</td>
      <td>106759</td>
      <td>Joseph Gordon-Levitt</td>
      <td>0.0</td>
      <td>deception|imprisonment|lawlessness|police officer|terrorist plot</td>
      <td>http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>Doug Walker</td>
      <td>Star Wars: Episode VII - The Force Awakens</td>
      <td>8</td>
      <td>143</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt5289954/?ref_=fn_tt_tt_1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.columns
```




    Index(['color', 'director_name', 'num_critic_for_reviews', 'duration',
           'director_facebook_likes', 'actor_3_facebook_likes', 'actor_2_name',
           'actor_1_facebook_likes', 'gross', 'genres', 'actor_1_name',
           'movie_title', 'num_voted_users', 'cast_total_facebook_likes',
           'actor_3_name', 'facenumber_in_poster', 'plot_keywords',
           'movie_imdb_link', 'num_user_for_reviews', 'language', 'country',
           'content_rating', 'budget', 'title_year', 'actor_2_facebook_likes',
           'imdb_score', 'aspect_ratio', 'movie_facebook_likes'],
          dtype='object')




```python
# 1.  director_name 컬럼의 값들 조회
df['director_name']
```




    0           James Cameron
    1          Gore Verbinski
    2              Sam Mendes
    3       Christopher Nolan
    4             Doug Walker
                  ...        
    4911          Scott Smith
    4912                  NaN
    4913     Benjamin Roberds
    4914          Daniel Hsia
    4915             Jon Gunn
    Name: director_name, Length: 4916, dtype: object




```python
df.director_name
```




    0           James Cameron
    1          Gore Verbinski
    2              Sam Mendes
    3       Christopher Nolan
    4             Doug Walker
                  ...        
    4911          Scott Smith
    4912                  NaN
    4913     Benjamin Roberds
    4914          Daniel Hsia
    4915             Jon Gunn
    Name: director_name, Length: 4916, dtype: object




```python
# 2. actor_1_name, actor_2_name, actor_3_name 컬럼의 값들 
df[["actor_1_name", "actor_2_name", "actor_3_name" ]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>actor_1_name</th>
      <th>actor_2_name</th>
      <th>actor_3_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CCH Pounder</td>
      <td>Joel David Moore</td>
      <td>Wes Studi</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Johnny Depp</td>
      <td>Orlando Bloom</td>
      <td>Jack Davenport</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Christoph Waltz</td>
      <td>Rory Kinnear</td>
      <td>Stephanie Sigman</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Tom Hardy</td>
      <td>Christian Bale</td>
      <td>Joseph Gordon-Levitt</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Doug Walker</td>
      <td>Rob Walker</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Eric Mabius</td>
      <td>Daphne Zuniga</td>
      <td>Crystal Lowe</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Natalie Zea</td>
      <td>Valorie Curry</td>
      <td>Sam Underwood</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Eva Boehnke</td>
      <td>Maxwell Moody</td>
      <td>David Chandler</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Alan Ruck</td>
      <td>Daniel Henney</td>
      <td>Eliza Coupe</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>John August</td>
      <td>Brian Herzlinger</td>
      <td>Jon Gunn</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 3 columns</p>
</div>




```python
df.columns[[1, 3, 4, 7]]
```




    Index(['director_name', 'duration', 'director_facebook_likes',
           'actor_1_facebook_likes'],
          dtype='object')




```python
# 3. 1, 3, 4, 7 번 컬럼 조회('director_name', 'duration', 'director_facebook_likes', 'actor_1_facebook_likes')
df[df.columns[[1, 3, 4, 7]]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>director_name</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_1_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>James Cameron</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Gore Verbinski</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>40000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sam Mendes</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>11000.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Christopher Nolan</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>27000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>131.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Scott Smith</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>637.0</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>NaN</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>841.0</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Benjamin Roberds</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Daniel Hsia</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>946.0</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Jon Gunn</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>86.0</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 4 columns</p>
</div>




```python
# 4. 1 ~ 5 번 컬럼 조회('director_name', 'num_critic_for_reviews', 'duration', director_facebook_likes', 'actor_3_facebook_likes')
df[df.columns[1:6]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Scott Smith</td>
      <td>1.0</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>318.0</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>NaN</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>319.0</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 5 columns</p>
</div>



### 다양한 열선택 기능을 제공하는 메소드들
- **`select_dtypes(include=[데이터타입,..], exclude=[데이터타입,..])`**
    - 전달한 데이터 타입의 열들을 조회. 
    - include : 조회할 열 데이터 타입
    - exclude : 제외하고 조회할 열 데이터 타입
- **`filter (items=[], like='', regex='')`**
   - 매개변수에 전달하는 열의 이름에 따라 조회
    - 각 매개변수중 하나만 사용할 수 있다.
    - **items = \[컬럼명들, ..\]**
        - 리스트와 일치하는 열들 조회
        - 이름이 일치 하지 않아도 Error 발생안함.
    - **like = '부분일치문자열'**
        - 전달한 문자열이 들어간 열들 조회
        - 부분일치 개념
    - **regex = '정규표현식'**
        - 정규 표현식을 이용해 열명의 패턴으로 조회


```python
grade.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 5 entries, id-1 to id-5
    Data columns (total 6 columns):
     #   Column  Non-Null Count  Dtype  
    ---  ------  --------------  -----  
     0   국어      5 non-null      int64  
     1   영어      5 non-null      int64  
     2   총점      5 non-null      int64  
     3   평균      5 non-null      float64
     4   합격여부    5 non-null      bool   
     5   통과여부2   5 non-null      object 
    dtypes: bool(1), float64(1), int64(3), object(1)
    memory usage: 417.0+ bytes
    


```python
# 데이터 타입으로 조회
## include: 조회하고 싶은 타입들을 지정.
grade.select_dtypes(include=['int64', 'bool'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>합격여부</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>False</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade.select_dtypes(include="int64")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
    </tr>
  </tbody>
</table>
</div>




```python
##  exclude: 조회 안할 타입들을 지정.
grade.select_dtypes(exclude=["object", "bool"])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade_result = pd.read_csv("saved_data/grade_result.csv", 
                           index_col=0)
grade_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>수학</th>
      <th>과학</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>70</td>
      <td>320</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>100</td>
      <td>370</td>
      <td>92</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>90</td>
      <td>345</td>
      <td>86</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>40</td>
      <td>40</td>
      <td>230</td>
      <td>57</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
      <td>280</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼을 중간에 삽입
grade_result.insert(loc=1, column="국어2", value=90)
```


```python
grade_result.insert(loc=4,         # 컬럼을 삽입할 위치 -> 순번으로 지정
                   column="수학2", # 컬럼이름
                   value=[100, 80, 55, 85, 67])
```


```python
grade_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
      <th>영어</th>
      <th>수학</th>
      <th>수학2</th>
      <th>과학</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
      <td>80</td>
      <td>70</td>
      <td>100</td>
      <td>70</td>
      <td>320</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>90</td>
      <td>100</td>
      <td>80</td>
      <td>100</td>
      <td>370</td>
      <td>92</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>55</td>
      <td>90</td>
      <td>345</td>
      <td>86</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
      <td>80</td>
      <td>40</td>
      <td>85</td>
      <td>40</td>
      <td>230</td>
      <td>57</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
      <td>60</td>
      <td>70</td>
      <td>67</td>
      <td>70</td>
      <td>280</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 없는 값 조회 Error 
# grade_result[['국어', '국어2', '국어3']]
```


```python
# 없는 값 filter 조회지 Error 안남
grade_result.filter(items=['국어', '국어2', '국어3'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼명에 "국어" 가 들어가는 컬럼들을 조회
grade_result.filter(like='국어')  
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ^:시작  .{2}: 두글자   $:끝
## 정규표현식과 일치하는 컬럼들을 조회
grade_result.filter(regex=r"^.{2}$")  
grade_result.filter(regex=r".+어\d?$")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
      <th>영어</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>90</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
      <td>75</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
      <td>60</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade_result
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
      <th>영어</th>
      <th>수학</th>
      <th>수학2</th>
      <th>과학</th>
      <th>총점</th>
      <th>평균</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
      <td>80</td>
      <td>70</td>
      <td>100</td>
      <td>70</td>
      <td>320</td>
      <td>80</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>90</td>
      <td>100</td>
      <td>80</td>
      <td>100</td>
      <td>370</td>
      <td>92</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
      <td>75</td>
      <td>90</td>
      <td>55</td>
      <td>90</td>
      <td>345</td>
      <td>86</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
      <td>80</td>
      <td>40</td>
      <td>85</td>
      <td>40</td>
      <td>230</td>
      <td>57</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
      <td>60</td>
      <td>70</td>
      <td>67</td>
      <td>70</td>
      <td>280</td>
      <td>70</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 컬럼의 순서를 변경 -> 원하는 순서로 조회
g = grade_result[['국어','국어2', '수학', '수학2', '영어', '과학', '평균', '총점']]
g
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>국어2</th>
      <th>수학</th>
      <th>수학2</th>
      <th>영어</th>
      <th>과학</th>
      <th>평균</th>
      <th>총점</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90</td>
      <td>70</td>
      <td>100</td>
      <td>80</td>
      <td>70</td>
      <td>80</td>
      <td>320</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>80</td>
      <td>90</td>
      <td>100</td>
      <td>92</td>
      <td>370</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>90</td>
      <td>90</td>
      <td>55</td>
      <td>75</td>
      <td>90</td>
      <td>86</td>
      <td>345</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>90</td>
      <td>40</td>
      <td>85</td>
      <td>80</td>
      <td>40</td>
      <td>57</td>
      <td>230</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>90</td>
      <td>70</td>
      <td>67</td>
      <td>60</td>
      <td>70</td>
      <td>70</td>
      <td>280</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>

다음은 movie dataframe을 이용해 아래 코드를 작성하시오.


```python
import pandas as pd

df = pd.read_csv('data/movie.csv')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Color</td>
      <td>Scott Smith</td>
      <td>1.0</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>318.0</td>
      <td>Daphne Zuniga</td>
      <td>637.0</td>
      <td>NaN</td>
      <td>Comedy|Drama</td>
      <td>...</td>
      <td>6.0</td>
      <td>English</td>
      <td>Canada</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2013.0</td>
      <td>470.0</td>
      <td>7.7</td>
      <td>NaN</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Color</td>
      <td>NaN</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>319.0</td>
      <td>Valorie Curry</td>
      <td>841.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Mystery|Thriller</td>
      <td>...</td>
      <td>359.0</td>
      <td>English</td>
      <td>USA</td>
      <td>TV-14</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>593.0</td>
      <td>7.5</td>
      <td>16.00</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Maxwell Moody</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Drama|Horror|Thriller</td>
      <td>...</td>
      <td>3.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>1400.0</td>
      <td>2013.0</td>
      <td>0.0</td>
      <td>6.3</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
      <td>Daniel Henney</td>
      <td>946.0</td>
      <td>10443.0</td>
      <td>Comedy|Drama|Romance</td>
      <td>...</td>
      <td>9.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>NaN</td>
      <td>2012.0</td>
      <td>719.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>Brian Herzlinger</td>
      <td>86.0</td>
      <td>85222.0</td>
      <td>Documentary</td>
      <td>...</td>
      <td>84.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>1100.0</td>
      <td>2004.0</td>
      <td>23.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 28 columns</p>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4916 entries, 0 to 4915
    Data columns (total 28 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   color                      4897 non-null   object 
     1   director_name              4814 non-null   object 
     2   num_critic_for_reviews     4867 non-null   float64
     3   duration                   4901 non-null   float64
     4   director_facebook_likes    4814 non-null   float64
     5   actor_3_facebook_likes     4893 non-null   float64
     6   actor_2_name               4903 non-null   object 
     7   actor_1_facebook_likes     4909 non-null   float64
     8   gross                      4054 non-null   float64
     9   genres                     4916 non-null   object 
     10  actor_1_name               4909 non-null   object 
     11  movie_title                4916 non-null   object 
     12  num_voted_users            4916 non-null   int64  
     13  cast_total_facebook_likes  4916 non-null   int64  
     14  actor_3_name               4893 non-null   object 
     15  facenumber_in_poster       4903 non-null   float64
     16  plot_keywords              4764 non-null   object 
     17  movie_imdb_link            4916 non-null   object 
     18  num_user_for_reviews       4895 non-null   float64
     19  language                   4902 non-null   object 
     20  country                    4911 non-null   object 
     21  content_rating             4616 non-null   object 
     22  budget                     4432 non-null   float64
     23  title_year                 4810 non-null   float64
     24  actor_2_facebook_likes     4903 non-null   float64
     25  imdb_score                 4916 non-null   float64
     26  aspect_ratio               4590 non-null   float64
     27  movie_facebook_likes       4916 non-null   int64  
    dtypes: float64(13), int64(3), object(12)
    memory usage: 1.1+ MB
    


```python
# 1. 정수형(int64) 컬럼만 조회
df.select_dtypes(include="int64")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>886204</td>
      <td>4834</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>471220</td>
      <td>48350</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>275868</td>
      <td>11700</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1144337</td>
      <td>106759</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>143</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>629</td>
      <td>2283</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>73839</td>
      <td>1753</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>38</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>1255</td>
      <td>2386</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>4285</td>
      <td>163</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 3 columns</p>
</div>




```python
# 2. 정수형(int64)과 실수형(float64) 타입을 제외한 컬럼들만 조회
df.select_dtypes(exclude=["int64", "float64"])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>actor_2_name</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>actor_3_name</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>Joel David Moore</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>CCH Pounder</td>
      <td>Avatar</td>
      <td>Wes Studi</td>
      <td>avatar|future|marine|native|paraplegic</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>Orlando Bloom</td>
      <td>Action|Adventure|Fantasy</td>
      <td>Johnny Depp</td>
      <td>Pirates of the Caribbean: At World's End</td>
      <td>Jack Davenport</td>
      <td>goddess|marriage ceremony|marriage proposal|pirate|singapore</td>
      <td>http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>Rory Kinnear</td>
      <td>Action|Adventure|Thriller</td>
      <td>Christoph Waltz</td>
      <td>Spectre</td>
      <td>Stephanie Sigman</td>
      <td>bomb|espionage|sequel|spy|terrorist</td>
      <td>http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>Christian Bale</td>
      <td>Action|Thriller</td>
      <td>Tom Hardy</td>
      <td>The Dark Knight Rises</td>
      <td>Joseph Gordon-Levitt</td>
      <td>deception|imprisonment|lawlessness|police officer|terrorist plot</td>
      <td>http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>Rob Walker</td>
      <td>Documentary</td>
      <td>Doug Walker</td>
      <td>Star Wars: Episode VII - The Force Awakens</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt5289954/?ref_=fn_tt_tt_1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Color</td>
      <td>Scott Smith</td>
      <td>Daphne Zuniga</td>
      <td>Comedy|Drama</td>
      <td>Eric Mabius</td>
      <td>Signed Sealed Delivered</td>
      <td>Crystal Lowe</td>
      <td>fraud|postal worker|prison|theft|trial</td>
      <td>http://www.imdb.com/title/tt3000844/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>Canada</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Color</td>
      <td>NaN</td>
      <td>Valorie Curry</td>
      <td>Crime|Drama|Mystery|Thriller</td>
      <td>Natalie Zea</td>
      <td>The Following</td>
      <td>Sam Underwood</td>
      <td>cult|fbi|hideout|prison escape|serial killer</td>
      <td>http://www.imdb.com/title/tt2071645/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>TV-14</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>Maxwell Moody</td>
      <td>Drama|Horror|Thriller</td>
      <td>Eva Boehnke</td>
      <td>A Plague So Pleasant</td>
      <td>David Chandler</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2107644/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>Daniel Henney</td>
      <td>Comedy|Drama|Romance</td>
      <td>Alan Ruck</td>
      <td>Shanghai Calling</td>
      <td>Eliza Coupe</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2070597/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>Brian Herzlinger</td>
      <td>Documentary</td>
      <td>John August</td>
      <td>My Date with Drew</td>
      <td>Jon Gunn</td>
      <td>actress name in title|crush|date|four word title|video camera</td>
      <td>http://www.imdb.com/title/tt0378407/?ref_=fn_tt_tt_1</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 12 columns</p>
</div>




```python
# 3. actor_1_name, actor_2_name, actor_3_name 컬럼의 값을 조회
df.filter(regex=r'actor_[123]_name')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>actor_2_name</th>
      <th>actor_1_name</th>
      <th>actor_3_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Joel David Moore</td>
      <td>CCH Pounder</td>
      <td>Wes Studi</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Orlando Bloom</td>
      <td>Johnny Depp</td>
      <td>Jack Davenport</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rory Kinnear</td>
      <td>Christoph Waltz</td>
      <td>Stephanie Sigman</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Christian Bale</td>
      <td>Tom Hardy</td>
      <td>Joseph Gordon-Levitt</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rob Walker</td>
      <td>Doug Walker</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Daphne Zuniga</td>
      <td>Eric Mabius</td>
      <td>Crystal Lowe</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Valorie Curry</td>
      <td>Natalie Zea</td>
      <td>Sam Underwood</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Maxwell Moody</td>
      <td>Eva Boehnke</td>
      <td>David Chandler</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Daniel Henney</td>
      <td>Alan Ruck</td>
      <td>Eliza Coupe</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Brian Herzlinger</td>
      <td>John August</td>
      <td>Jon Gunn</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 3 columns</p>
</div>




```python
# 4. actor_1_facebook_likes, actor_1_name 컬럼의 값을 조회
df.filter(like="actor_1")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>actor_1_facebook_likes</th>
      <th>actor_1_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1000.0</td>
      <td>CCH Pounder</td>
    </tr>
    <tr>
      <th>1</th>
      <td>40000.0</td>
      <td>Johnny Depp</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11000.0</td>
      <td>Christoph Waltz</td>
    </tr>
    <tr>
      <th>3</th>
      <td>27000.0</td>
      <td>Tom Hardy</td>
    </tr>
    <tr>
      <th>4</th>
      <td>131.0</td>
      <td>Doug Walker</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>637.0</td>
      <td>Eric Mabius</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>841.0</td>
      <td>Natalie Zea</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>0.0</td>
      <td>Eva Boehnke</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>946.0</td>
      <td>Alan Ruck</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>86.0</td>
      <td>John August</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 2 columns</p>
</div>




```python
# 5. color, director 컬럼을 조회. 없는 컬럼명이라도 에러가 안나도록 조회하시오.
df.filter(items=['color', 'director'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 1 columns</p>
</div>




```python
# 6. movie가 들어가는 컬럼들을 조회.
df.filter(like='movie')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>movie_title</th>
      <th>movie_imdb_link</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Avatar</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Pirates of the Caribbean: At World's End</td>
      <td>http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Spectre</td>
      <td>http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>The Dark Knight Rises</td>
      <td>http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Star Wars: Episode VII - The Force Awakens</td>
      <td>http://www.imdb.com/title/tt5289954/?ref_=fn_tt_tt_1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Signed Sealed Delivered</td>
      <td>http://www.imdb.com/title/tt3000844/?ref_=fn_tt_tt_1</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>The Following</td>
      <td>http://www.imdb.com/title/tt2071645/?ref_=fn_tt_tt_1</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>A Plague So Pleasant</td>
      <td>http://www.imdb.com/title/tt2107644/?ref_=fn_tt_tt_1</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Shanghai Calling</td>
      <td>http://www.imdb.com/title/tt2070597/?ref_=fn_tt_tt_1</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>My Date with Drew</td>
      <td>http://www.imdb.com/title/tt0378407/?ref_=fn_tt_tt_1</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 3 columns</p>
</div>




```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>



## 행 조회

- **loc** :  index 이름으로 조회
- **iloc** : 행 순번으로 조회


### loc indexer
- index name으로 조회
- **`DF.loc[ index이름 ]`**
    - 한 행 조회.
    - 조회할 행 index 이름(레이블) 전달
    - 이름이 문자열이면 " " 문자열표기법으로 전달. 정수이며 정수표기법으로 전달한다.
- **`DF.loc[ index이름 리스트 ]`**
    - 여러 행 조회. 
    - 팬시 인덱스
    - 조회할 행 index 이름(레이블) 리스트 전달
- **`DF.loc[start index이름 : end index이름: step]`**
    - 슬라이싱 지원
    - end index 이름의 행까지 포함한다.
- **`DF.loc[index이름 , 컬럼이름]`**
    - 행과 열 조회
    - 둘다 이름으로 지정해야 함.


```python
grade
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade.loc['id-3'] # series. index이름: 컬럼명
```




    국어         90
    영어         75
    총점        165
    평균       82.5
    합격여부     True
    통과여부2      합격
    Name: id-3, dtype: object




```python
grade.loc[['id-2', 'id-4']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade.loc['id-2':'id-4']
grade.loc['id-2':'id-4':2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade['id-5':'id-2':-1]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 행, 열을 같이 조회 
## loc[행이름, 열이름] : 행/열 - indexname, fancy indexing, slicing
grade.loc['id-2','총점']
grade.loc[['id-2', 'id-3'] ,  '총점']
grade.loc[['id-2', 'id-5'], ['총점', '평균']]
grade.loc['id-1':'id-4', ['국어', '평균']]
grade.loc['id-2':'id-4', '영어':'합격여부']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-2</th>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>80</td>
      <td>150</td>
      <td>75.0</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
# loc index: 행/열 모두 이름으로 조회. (순번은 사용못함.)
# grade.loc[1:3]
# grade.loc['id-3', 1]

## indexer 안에서 행, 열을 같이 조회하는 것은 loc, iloc indexer만 가능
# grade['국어', 'id-2']
# grade['id-2', '국어']
grade['국어']['id-2']
grade[['국어', '영어']].loc['id-2']
```




    국어    80
    영어    90
    Name: id-2, dtype: int64



### iloc 
- index(행 순번)으로 조회
- **`DF.iloc[행번호]`**
    - 한 행 조회.
    - 조회할 행 번호 전달
- **`DF.iloc[ 행번호 리스트 ]`**
    - 여러 행 조회.
    - 조회할 행 번호 리스트 전달
- **`DF.iloc[start 행번호: stop 행번호: step]`**
    - 슬라이싱 지원
    - stop 행번호 포함 안함.
- **`DF.iloc[행번호 , 열번호]`**
    - 행과 열 조회
    - 행열 모두 순번으로 지정


```python
grade.iloc[0]
grade.iloc[2]
grade.iloc[[0, 2, 3]]
grade.iloc[1:3]
grade.iloc[-1]
grade.iloc[1:-1]
grade.iloc[1, 2]  #iloc -> 행/열 모두 순번(index)으로 조회.
grade.iloc[[2, 4], 2]
grade.iloc[:4, [0, 1]]
grade.iloc[1:3, 1:]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-2</th>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
# grade['국어']       => select 국어
# grade.loc['id-1']   => where절
```

<b style='font-size:2.2em'>TODO</b>
- movie dataframe을 이용해 loc과 iloc관련해 다음을 작성


```python
import pandas as pd
df = pd.read_csv('data/movie.csv')
df.shape
```




    (4916, 28)




```python
# 1.  movie_title 컬럼을 index명으로 지정
df.set_index("movie_title", inplace=True)
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Avatar</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>Pirates of the Caribbean: At World's End</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Spectre</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>The Dark Knight Rises</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>Star Wars: Episode VII - The Force Awakens</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Signed Sealed Delivered</th>
      <td>Color</td>
      <td>Scott Smith</td>
      <td>1.0</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>318.0</td>
      <td>Daphne Zuniga</td>
      <td>637.0</td>
      <td>NaN</td>
      <td>Comedy|Drama</td>
      <td>...</td>
      <td>6.0</td>
      <td>English</td>
      <td>Canada</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2013.0</td>
      <td>470.0</td>
      <td>7.7</td>
      <td>NaN</td>
      <td>84</td>
    </tr>
    <tr>
      <th>The Following</th>
      <td>Color</td>
      <td>NaN</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>319.0</td>
      <td>Valorie Curry</td>
      <td>841.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Mystery|Thriller</td>
      <td>...</td>
      <td>359.0</td>
      <td>English</td>
      <td>USA</td>
      <td>TV-14</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>593.0</td>
      <td>7.5</td>
      <td>16.00</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>A Plague So Pleasant</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Maxwell Moody</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Drama|Horror|Thriller</td>
      <td>...</td>
      <td>3.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>1400.0</td>
      <td>2013.0</td>
      <td>0.0</td>
      <td>6.3</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Shanghai Calling</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
      <td>Daniel Henney</td>
      <td>946.0</td>
      <td>10443.0</td>
      <td>Comedy|Drama|Romance</td>
      <td>...</td>
      <td>9.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>NaN</td>
      <td>2012.0</td>
      <td>719.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>660</td>
    </tr>
    <tr>
      <th>My Date with Drew</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>Brian Herzlinger</td>
      <td>86.0</td>
      <td>85222.0</td>
      <td>Documentary</td>
      <td>...</td>
      <td>84.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>1100.0</td>
      <td>2004.0</td>
      <td>23.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 27 columns</p>
</div>




```python
# 2.  행이름이 Avatar인 행 조회
df.loc['Avatar']
```




    color                                                                    Color
    director_name                                                    James Cameron
    num_critic_for_reviews                                                   723.0
    duration                                                                 178.0
    director_facebook_likes                                                    0.0
    actor_3_facebook_likes                                                   855.0
    actor_2_name                                                  Joel David Moore
    actor_1_facebook_likes                                                  1000.0
    gross                                                              760505847.0
    genres                                         Action|Adventure|Fantasy|Sci-Fi
    actor_1_name                                                       CCH Pounder
    num_voted_users                                                         886204
    cast_total_facebook_likes                                                 4834
    actor_3_name                                                         Wes Studi
    facenumber_in_poster                                                       0.0
    plot_keywords                           avatar|future|marine|native|paraplegic
    movie_imdb_link              http://www.imdb.com/title/tt0499549/?ref_=fn_t...
    num_user_for_reviews                                                    3054.0
    language                                                               English
    country                                                                    USA
    content_rating                                                           PG-13
    budget                                                             237000000.0
    title_year                                                              2009.0
    actor_2_facebook_likes                                                   936.0
    imdb_score                                                                 7.9
    aspect_ratio                                                              1.78
    movie_facebook_likes                                                     33000
    Name: Avatar, dtype: object




```python
# 3.  행이름이 Spider-Man 3, The Avengers, Titanic 인 행 조회
# fancy indexing
df.loc[[ 'Spider-Man 3','The Avengers', 'Titanic']].T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>movie_title</th>
      <th>Spider-Man 3</th>
      <th>The Avengers</th>
      <th>Titanic</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>color</th>
      <td>Color</td>
      <td>Color</td>
      <td>Color</td>
    </tr>
    <tr>
      <th>director_name</th>
      <td>Sam Raimi</td>
      <td>Joss Whedon</td>
      <td>James Cameron</td>
    </tr>
    <tr>
      <th>num_critic_for_reviews</th>
      <td>392.0</td>
      <td>703.0</td>
      <td>315.0</td>
    </tr>
    <tr>
      <th>duration</th>
      <td>156.0</td>
      <td>173.0</td>
      <td>194.0</td>
    </tr>
    <tr>
      <th>director_facebook_likes</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>actor_3_facebook_likes</th>
      <td>4000.0</td>
      <td>19000.0</td>
      <td>794.0</td>
    </tr>
    <tr>
      <th>actor_2_name</th>
      <td>James Franco</td>
      <td>Robert Downey Jr.</td>
      <td>Kate Winslet</td>
    </tr>
    <tr>
      <th>actor_1_facebook_likes</th>
      <td>24000.0</td>
      <td>26000.0</td>
      <td>29000.0</td>
    </tr>
    <tr>
      <th>gross</th>
      <td>336530303.0</td>
      <td>623279547.0</td>
      <td>658672302.0</td>
    </tr>
    <tr>
      <th>genres</th>
      <td>Action|Adventure|Romance</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>Drama|Romance</td>
    </tr>
    <tr>
      <th>actor_1_name</th>
      <td>J.K. Simmons</td>
      <td>Chris Hemsworth</td>
      <td>Leonardo DiCaprio</td>
    </tr>
    <tr>
      <th>num_voted_users</th>
      <td>383056</td>
      <td>995415</td>
      <td>793059</td>
    </tr>
    <tr>
      <th>cast_total_facebook_likes</th>
      <td>46055</td>
      <td>87697</td>
      <td>45223</td>
    </tr>
    <tr>
      <th>actor_3_name</th>
      <td>Kirsten Dunst</td>
      <td>Scarlett Johansson</td>
      <td>Gloria Stuart</td>
    </tr>
    <tr>
      <th>facenumber_in_poster</th>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>plot_keywords</th>
      <td>sandman|spider man|symbiote|venom|villain</td>
      <td>alien invasion|assassin|battle|iron man|soldier</td>
      <td>artist|love|ship|titanic|wet</td>
    </tr>
    <tr>
      <th>movie_imdb_link</th>
      <td>http://www.imdb.com/title/tt0413300/?ref_=fn_t...</td>
      <td>http://www.imdb.com/title/tt0848228/?ref_=fn_t...</td>
      <td>http://www.imdb.com/title/tt0120338/?ref_=fn_t...</td>
    </tr>
    <tr>
      <th>num_user_for_reviews</th>
      <td>1902.0</td>
      <td>1722.0</td>
      <td>2528.0</td>
    </tr>
    <tr>
      <th>language</th>
      <td>English</td>
      <td>English</td>
      <td>English</td>
    </tr>
    <tr>
      <th>country</th>
      <td>USA</td>
      <td>USA</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>content_rating</th>
      <td>PG-13</td>
      <td>PG-13</td>
      <td>PG-13</td>
    </tr>
    <tr>
      <th>budget</th>
      <td>258000000.0</td>
      <td>220000000.0</td>
      <td>200000000.0</td>
    </tr>
    <tr>
      <th>title_year</th>
      <td>2007.0</td>
      <td>2012.0</td>
      <td>1997.0</td>
    </tr>
    <tr>
      <th>actor_2_facebook_likes</th>
      <td>11000.0</td>
      <td>21000.0</td>
      <td>14000.0</td>
    </tr>
    <tr>
      <th>imdb_score</th>
      <td>6.2</td>
      <td>8.1</td>
      <td>7.7</td>
    </tr>
    <tr>
      <th>aspect_ratio</th>
      <td>2.35</td>
      <td>1.85</td>
      <td>2.35</td>
    </tr>
    <tr>
      <th>movie_facebook_likes</th>
      <td>0</td>
      <td>123000</td>
      <td>26000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4.  행이름 Spectre ~ Robin Hood 까지 범위로 조회
## slicing : 범위로 조회
### end index 포함(이름으로 조회)
df.loc['Spectre':"Robin Hood"] 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Spectre</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>Tangled</th>
      <td>Color</td>
      <td>Nathan Greno</td>
      <td>324.0</td>
      <td>100.0</td>
      <td>15.0</td>
      <td>284.0</td>
      <td>Donna Murphy</td>
      <td>799.0</td>
      <td>200807262.0</td>
      <td>Adventure|Animation|Comedy|Family|Fantasy|Musi...</td>
      <td>...</td>
      <td>387.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>260000000.0</td>
      <td>2010.0</td>
      <td>553.0</td>
      <td>7.8</td>
      <td>1.85</td>
      <td>29000</td>
    </tr>
    <tr>
      <th>Quantum of Solace</th>
      <td>Color</td>
      <td>Marc Forster</td>
      <td>403.0</td>
      <td>106.0</td>
      <td>395.0</td>
      <td>393.0</td>
      <td>Mathieu Amalric</td>
      <td>451.0</td>
      <td>168368427.0</td>
      <td>Action|Adventure</td>
      <td>...</td>
      <td>1243.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>200000000.0</td>
      <td>2008.0</td>
      <td>412.0</td>
      <td>6.7</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Avengers</th>
      <td>Color</td>
      <td>Joss Whedon</td>
      <td>703.0</td>
      <td>173.0</td>
      <td>0.0</td>
      <td>19000.0</td>
      <td>Robert Downey Jr.</td>
      <td>26000.0</td>
      <td>623279547.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>1722.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>220000000.0</td>
      <td>2012.0</td>
      <td>21000.0</td>
      <td>8.1</td>
      <td>1.85</td>
      <td>123000</td>
    </tr>
    <tr>
      <th>Robin Hood</th>
      <td>Color</td>
      <td>Ridley Scott</td>
      <td>343.0</td>
      <td>156.0</td>
      <td>0.0</td>
      <td>738.0</td>
      <td>William Hurt</td>
      <td>891.0</td>
      <td>105219735.0</td>
      <td>Action|Adventure|Drama|History</td>
      <td>...</td>
      <td>546.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>200000000.0</td>
      <td>2010.0</td>
      <td>882.0</td>
      <td>6.7</td>
      <td>2.35</td>
      <td>17000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>




```python
# 5.  행이름이 John Carter 이고 열이름이 director_name 인 값 조회 - John Carter의 감독이름 
df.loc["John Carter" , 'director_name']
# df.loc["John Carter"]['director_name']
```




    'Andrew Stanton'




```python
# 6.  1번행 조회
df.iloc[0]
```




    color                                                                    Color
    director_name                                                    James Cameron
    num_critic_for_reviews                                                   723.0
    duration                                                                 178.0
    director_facebook_likes                                                    0.0
    actor_3_facebook_likes                                                   855.0
    actor_2_name                                                  Joel David Moore
    actor_1_facebook_likes                                                  1000.0
    gross                                                              760505847.0
    genres                                         Action|Adventure|Fantasy|Sci-Fi
    actor_1_name                                                       CCH Pounder
    num_voted_users                                                         886204
    cast_total_facebook_likes                                                 4834
    actor_3_name                                                         Wes Studi
    facenumber_in_poster                                                       0.0
    plot_keywords                           avatar|future|marine|native|paraplegic
    movie_imdb_link              http://www.imdb.com/title/tt0499549/?ref_=fn_t...
    num_user_for_reviews                                                    3054.0
    language                                                               English
    country                                                                    USA
    content_rating                                                           PG-13
    budget                                                             237000000.0
    title_year                                                              2009.0
    actor_2_facebook_likes                                                   936.0
    imdb_score                                                                 7.9
    aspect_ratio                                                              1.78
    movie_facebook_likes                                                     33000
    Name: Avatar, dtype: object




```python
# 7.  마지막 행 조회
df.iloc[-1]
```




    color                                                                    Color
    director_name                                                         Jon Gunn
    num_critic_for_reviews                                                    43.0
    duration                                                                  90.0
    director_facebook_likes                                                   16.0
    actor_3_facebook_likes                                                    16.0
    actor_2_name                                                  Brian Herzlinger
    actor_1_facebook_likes                                                    86.0
    gross                                                                  85222.0
    genres                                                             Documentary
    actor_1_name                                                       John August
    num_voted_users                                                           4285
    cast_total_facebook_likes                                                  163
    actor_3_name                                                          Jon Gunn
    facenumber_in_poster                                                       0.0
    plot_keywords                actress name in title|crush|date|four word tit...
    movie_imdb_link              http://www.imdb.com/title/tt0378407/?ref_=fn_t...
    num_user_for_reviews                                                      84.0
    language                                                               English
    country                                                                    USA
    content_rating                                                              PG
    budget                                                                  1100.0
    title_year                                                              2004.0
    actor_2_facebook_likes                                                    23.0
    imdb_score                                                                 6.6
    aspect_ratio                                                              1.85
    movie_facebook_likes                                                       456
    Name: My Date with Drew, dtype: object




```python
# 8.  1, 2, 5, 6, 9 번행 조회
## fancy index
df.iloc[[0, 1, 4, 5, 8]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Avatar</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>Pirates of the Caribbean: At World's End</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Star Wars: Episode VII - The Force Awakens</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>John Carter</th>
      <td>Color</td>
      <td>Andrew Stanton</td>
      <td>462.0</td>
      <td>132.0</td>
      <td>475.0</td>
      <td>530.0</td>
      <td>Samantha Morton</td>
      <td>640.0</td>
      <td>73058679.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>738.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>263700000.0</td>
      <td>2012.0</td>
      <td>632.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>24000</td>
    </tr>
    <tr>
      <th>Avengers: Age of Ultron</th>
      <td>Color</td>
      <td>Joss Whedon</td>
      <td>635.0</td>
      <td>141.0</td>
      <td>0.0</td>
      <td>19000.0</td>
      <td>Robert Downey Jr.</td>
      <td>26000.0</td>
      <td>458991599.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>1117.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2015.0</td>
      <td>21000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>118000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>




```python
# 9.  10 ~ 20 행 조회
## slicing
df.iloc[9:20]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Harry Potter and the Half-Blood Prince</th>
      <td>Color</td>
      <td>David Yates</td>
      <td>375.0</td>
      <td>153.0</td>
      <td>282.0</td>
      <td>10000.0</td>
      <td>Daniel Radcliffe</td>
      <td>25000.0</td>
      <td>301956980.0</td>
      <td>Adventure|Family|Fantasy|Mystery</td>
      <td>...</td>
      <td>973.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG</td>
      <td>250000000.0</td>
      <td>2009.0</td>
      <td>11000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>10000</td>
    </tr>
    <tr>
      <th>Batman v Superman: Dawn of Justice</th>
      <td>Color</td>
      <td>Zack Snyder</td>
      <td>673.0</td>
      <td>183.0</td>
      <td>0.0</td>
      <td>2000.0</td>
      <td>Lauren Cohan</td>
      <td>15000.0</td>
      <td>330249062.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>3018.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2016.0</td>
      <td>4000.0</td>
      <td>6.9</td>
      <td>2.35</td>
      <td>197000</td>
    </tr>
    <tr>
      <th>Superman Returns</th>
      <td>Color</td>
      <td>Bryan Singer</td>
      <td>434.0</td>
      <td>169.0</td>
      <td>0.0</td>
      <td>903.0</td>
      <td>Marlon Brando</td>
      <td>18000.0</td>
      <td>200069408.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>2367.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>209000000.0</td>
      <td>2006.0</td>
      <td>10000.0</td>
      <td>6.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Quantum of Solace</th>
      <td>Color</td>
      <td>Marc Forster</td>
      <td>403.0</td>
      <td>106.0</td>
      <td>395.0</td>
      <td>393.0</td>
      <td>Mathieu Amalric</td>
      <td>451.0</td>
      <td>168368427.0</td>
      <td>Action|Adventure</td>
      <td>...</td>
      <td>1243.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>200000000.0</td>
      <td>2008.0</td>
      <td>412.0</td>
      <td>6.7</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Pirates of the Caribbean: Dead Man's Chest</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>313.0</td>
      <td>151.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>423032628.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1832.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>225000000.0</td>
      <td>2006.0</td>
      <td>5000.0</td>
      <td>7.3</td>
      <td>2.35</td>
      <td>5000</td>
    </tr>
    <tr>
      <th>The Lone Ranger</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>450.0</td>
      <td>150.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Ruth Wilson</td>
      <td>40000.0</td>
      <td>89289910.0</td>
      <td>Action|Adventure|Western</td>
      <td>...</td>
      <td>711.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>215000000.0</td>
      <td>2013.0</td>
      <td>2000.0</td>
      <td>6.5</td>
      <td>2.35</td>
      <td>48000</td>
    </tr>
    <tr>
      <th>Man of Steel</th>
      <td>Color</td>
      <td>Zack Snyder</td>
      <td>733.0</td>
      <td>143.0</td>
      <td>0.0</td>
      <td>748.0</td>
      <td>Christopher Meloni</td>
      <td>15000.0</td>
      <td>291021565.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>2536.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>225000000.0</td>
      <td>2013.0</td>
      <td>3000.0</td>
      <td>7.2</td>
      <td>2.35</td>
      <td>118000</td>
    </tr>
    <tr>
      <th>The Chronicles of Narnia: Prince Caspian</th>
      <td>Color</td>
      <td>Andrew Adamson</td>
      <td>258.0</td>
      <td>150.0</td>
      <td>80.0</td>
      <td>201.0</td>
      <td>Pierfrancesco Favino</td>
      <td>22000.0</td>
      <td>141614023.0</td>
      <td>Action|Adventure|Family|Fantasy</td>
      <td>...</td>
      <td>438.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>225000000.0</td>
      <td>2008.0</td>
      <td>216.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Avengers</th>
      <td>Color</td>
      <td>Joss Whedon</td>
      <td>703.0</td>
      <td>173.0</td>
      <td>0.0</td>
      <td>19000.0</td>
      <td>Robert Downey Jr.</td>
      <td>26000.0</td>
      <td>623279547.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>1722.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>220000000.0</td>
      <td>2012.0</td>
      <td>21000.0</td>
      <td>8.1</td>
      <td>1.85</td>
      <td>123000</td>
    </tr>
    <tr>
      <th>Pirates of the Caribbean: On Stranger Tides</th>
      <td>Color</td>
      <td>Rob Marshall</td>
      <td>448.0</td>
      <td>136.0</td>
      <td>252.0</td>
      <td>1000.0</td>
      <td>Sam Claflin</td>
      <td>40000.0</td>
      <td>241063875.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>484.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2011.0</td>
      <td>11000.0</td>
      <td>6.7</td>
      <td>2.35</td>
      <td>58000</td>
    </tr>
    <tr>
      <th>Men in Black 3</th>
      <td>Color</td>
      <td>Barry Sonnenfeld</td>
      <td>451.0</td>
      <td>106.0</td>
      <td>188.0</td>
      <td>718.0</td>
      <td>Michael Stuhlbarg</td>
      <td>10000.0</td>
      <td>179020854.0</td>
      <td>Action|Adventure|Comedy|Family|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>341.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>225000000.0</td>
      <td>2012.0</td>
      <td>816.0</td>
      <td>6.8</td>
      <td>1.85</td>
      <td>40000</td>
    </tr>
  </tbody>
</table>
<p>11 rows × 27 columns</p>
</div>




```python
# 10.  movie dataframe에서 5 ~ 10 행의 color, director_name, num_critic_for_reviews 컬럼(0,1,2번째 컬럼)을 iloc을 이용해 조회
df.iloc[4:10, [0, 1, 2]]  ##  컬럼도 순번(index)로 지정.
df.iloc[4:10, :3]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Star Wars: Episode VII - The Force Awakens</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>John Carter</th>
      <td>Color</td>
      <td>Andrew Stanton</td>
      <td>462.0</td>
    </tr>
    <tr>
      <th>Spider-Man 3</th>
      <td>Color</td>
      <td>Sam Raimi</td>
      <td>392.0</td>
    </tr>
    <tr>
      <th>Tangled</th>
      <td>Color</td>
      <td>Nathan Greno</td>
      <td>324.0</td>
    </tr>
    <tr>
      <th>Avengers: Age of Ultron</th>
      <td>Color</td>
      <td>Joss Whedon</td>
      <td>635.0</td>
    </tr>
    <tr>
      <th>Harry Potter and the Half-Blood Prince</th>
      <td>Color</td>
      <td>David Yates</td>
      <td>375.0</td>
    </tr>
  </tbody>
</table>
</div>



## Boolean indexing을 이용한 조회
- 원하는 조건을 만족하는 행, 열을 조회한다.

- **`DataFrame객체[조건], DataFrame객체.loc[조건]`**
    - 조건이 True인 행만 조회
    - 열까지 선택시
        - `DataFrame객체[조건][열]`
        - `DataFrame객체.loc[조건, 열]`
- **iloc indexer**는 boolean indexing을 **지원하지 않는다.**

> - 논리연산자
> |논리연산자|설명|
> |:-:|-|
> |&|and연산|
> |\||or연산|
> |~|not 연산|

> - 논리연산자의 피연산자들은 반드시 ( )로 묶어준다.
> - 파이썬과는 다르게 `and`, `or`, `not` 예약어는 사용할 수 없다.


```python
df[열, 행]
df.loc[행, 열]
```


```python
# df['color'].value_counts()
## 조건이 True인 행이 조회 ==> sql select의 where절
df[df['color'] == 'Color']  
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>actor_3_name</th>
      <th>facenumber_in_poster</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>CCH Pounder</td>
      <td>Avatar</td>
      <td>886204</td>
      <td>4834</td>
      <td>Wes Studi</td>
      <td>0.0</td>
      <td>avatar|future|marine|native|paraplegic</td>
      <td>http://www.imdb.com/title/tt0499549/?ref_=fn_tt_tt_1</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>Johnny Depp</td>
      <td>Pirates of the Caribbean: At World's End</td>
      <td>471220</td>
      <td>48350</td>
      <td>Jack Davenport</td>
      <td>0.0</td>
      <td>goddess|marriage ceremony|marriage proposal|pirate|singapore</td>
      <td>http://www.imdb.com/title/tt0449088/?ref_=fn_tt_tt_1</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>Christoph Waltz</td>
      <td>Spectre</td>
      <td>275868</td>
      <td>11700</td>
      <td>Stephanie Sigman</td>
      <td>1.0</td>
      <td>bomb|espionage|sequel|spy|terrorist</td>
      <td>http://www.imdb.com/title/tt2379713/?ref_=fn_tt_tt_1</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>Tom Hardy</td>
      <td>The Dark Knight Rises</td>
      <td>1144337</td>
      <td>106759</td>
      <td>Joseph Gordon-Levitt</td>
      <td>0.0</td>
      <td>deception|imprisonment|lawlessness|police officer|terrorist plot</td>
      <td>http://www.imdb.com/title/tt1345836/?ref_=fn_tt_tt_1</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Color</td>
      <td>Andrew Stanton</td>
      <td>462.0</td>
      <td>132.0</td>
      <td>475.0</td>
      <td>530.0</td>
      <td>Samantha Morton</td>
      <td>640.0</td>
      <td>73058679.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>Daryl Sabara</td>
      <td>John Carter</td>
      <td>212204</td>
      <td>1873</td>
      <td>Polly Walker</td>
      <td>1.0</td>
      <td>alien|american civil war|male nipple|mars|princess</td>
      <td>http://www.imdb.com/title/tt0401729/?ref_=fn_tt_tt_1</td>
      <td>738.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>263700000.0</td>
      <td>2012.0</td>
      <td>632.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>24000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Color</td>
      <td>Scott Smith</td>
      <td>1.0</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>318.0</td>
      <td>Daphne Zuniga</td>
      <td>637.0</td>
      <td>NaN</td>
      <td>Comedy|Drama</td>
      <td>Eric Mabius</td>
      <td>Signed Sealed Delivered</td>
      <td>629</td>
      <td>2283</td>
      <td>Crystal Lowe</td>
      <td>2.0</td>
      <td>fraud|postal worker|prison|theft|trial</td>
      <td>http://www.imdb.com/title/tt3000844/?ref_=fn_tt_tt_1</td>
      <td>6.0</td>
      <td>English</td>
      <td>Canada</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2013.0</td>
      <td>470.0</td>
      <td>7.7</td>
      <td>NaN</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Color</td>
      <td>NaN</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>319.0</td>
      <td>Valorie Curry</td>
      <td>841.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Mystery|Thriller</td>
      <td>Natalie Zea</td>
      <td>The Following</td>
      <td>73839</td>
      <td>1753</td>
      <td>Sam Underwood</td>
      <td>1.0</td>
      <td>cult|fbi|hideout|prison escape|serial killer</td>
      <td>http://www.imdb.com/title/tt2071645/?ref_=fn_tt_tt_1</td>
      <td>359.0</td>
      <td>English</td>
      <td>USA</td>
      <td>TV-14</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>593.0</td>
      <td>7.5</td>
      <td>16.00</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Maxwell Moody</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Drama|Horror|Thriller</td>
      <td>Eva Boehnke</td>
      <td>A Plague So Pleasant</td>
      <td>38</td>
      <td>0</td>
      <td>David Chandler</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2107644/?ref_=fn_tt_tt_1</td>
      <td>3.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>1400.0</td>
      <td>2013.0</td>
      <td>0.0</td>
      <td>6.3</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
      <td>Daniel Henney</td>
      <td>946.0</td>
      <td>10443.0</td>
      <td>Comedy|Drama|Romance</td>
      <td>Alan Ruck</td>
      <td>Shanghai Calling</td>
      <td>1255</td>
      <td>2386</td>
      <td>Eliza Coupe</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt2070597/?ref_=fn_tt_tt_1</td>
      <td>9.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>NaN</td>
      <td>2012.0</td>
      <td>719.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>Brian Herzlinger</td>
      <td>86.0</td>
      <td>85222.0</td>
      <td>Documentary</td>
      <td>John August</td>
      <td>My Date with Drew</td>
      <td>4285</td>
      <td>163</td>
      <td>Jon Gunn</td>
      <td>0.0</td>
      <td>actress name in title|crush|date|four word title|video camera</td>
      <td>http://www.imdb.com/title/tt0378407/?ref_=fn_tt_tt_1</td>
      <td>84.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>1100.0</td>
      <td>2004.0</td>
      <td>23.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4693 rows × 28 columns</p>
</div>




```python
df.loc[df['color']=='Black and White']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>actor_1_name</th>
      <th>movie_title</th>
      <th>num_voted_users</th>
      <th>cast_total_facebook_likes</th>
      <th>actor_3_name</th>
      <th>facenumber_in_poster</th>
      <th>plot_keywords</th>
      <th>movie_imdb_link</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>111</th>
      <td>Black and White</td>
      <td>Michael Bay</td>
      <td>191.0</td>
      <td>184.0</td>
      <td>0.0</td>
      <td>691.0</td>
      <td>Jaime King</td>
      <td>3000.0</td>
      <td>198539855.0</td>
      <td>Action|Drama|History|Romance|War</td>
      <td>Jennifer Garner</td>
      <td>Pearl Harbor</td>
      <td>254111</td>
      <td>5401</td>
      <td>Mako</td>
      <td>0.0</td>
      <td>air raid|black smoke|japanese military|japanese navy|sunday</td>
      <td>http://www.imdb.com/title/tt0213149/?ref_=fn_tt_tt_1</td>
      <td>1999.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>140000000.0</td>
      <td>2001.0</td>
      <td>961.0</td>
      <td>6.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Black and White</td>
      <td>Lee Tamahori</td>
      <td>264.0</td>
      <td>133.0</td>
      <td>93.0</td>
      <td>746.0</td>
      <td>Colin Salmon</td>
      <td>769.0</td>
      <td>160201106.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>Toby Stephens</td>
      <td>Die Another Day</td>
      <td>169914</td>
      <td>2538</td>
      <td>Rick Yune</td>
      <td>0.0</td>
      <td>catfight|clinic|colonel|diamond|patricide</td>
      <td>http://www.imdb.com/title/tt0246460/?ref_=fn_tt_tt_1</td>
      <td>1185.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>142000000.0</td>
      <td>2002.0</td>
      <td>766.0</td>
      <td>6.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>254</th>
      <td>Black and White</td>
      <td>Martin Scorsese</td>
      <td>267.0</td>
      <td>170.0</td>
      <td>17000.0</td>
      <td>827.0</td>
      <td>Adam Scott</td>
      <td>29000.0</td>
      <td>102608827.0</td>
      <td>Biography|Drama</td>
      <td>Leonardo DiCaprio</td>
      <td>The Aviator</td>
      <td>264318</td>
      <td>34582</td>
      <td>Frances Conroy</td>
      <td>0.0</td>
      <td>1920s|aviation|fight|spruce goose|test flight</td>
      <td>http://www.imdb.com/title/tt0338751/?ref_=fn_tt_tt_1</td>
      <td>799.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>110000000.0</td>
      <td>2004.0</td>
      <td>3000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>269</th>
      <td>Black and White</td>
      <td>Michael Mann</td>
      <td>174.0</td>
      <td>165.0</td>
      <td>0.0</td>
      <td>780.0</td>
      <td>Jada Pinkett Smith</td>
      <td>10000.0</td>
      <td>58183966.0</td>
      <td>Biography|Drama|Sport</td>
      <td>Will Smith</td>
      <td>Ali</td>
      <td>79186</td>
      <td>14196</td>
      <td>Joe Morton</td>
      <td>1.0</td>
      <td>african american protagonist|african americans|boxing gym|gym|rumble in the jungle</td>
      <td>http://www.imdb.com/title/tt0248667/?ref_=fn_tt_tt_1</td>
      <td>386.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>107000000.0</td>
      <td>2001.0</td>
      <td>851.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>283</th>
      <td>Black and White</td>
      <td>Martin Campbell</td>
      <td>400.0</td>
      <td>144.0</td>
      <td>258.0</td>
      <td>834.0</td>
      <td>Tobias Menzies</td>
      <td>6000.0</td>
      <td>167007184.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>Eva Green</td>
      <td>Casino Royale</td>
      <td>470483</td>
      <td>9125</td>
      <td>Ivana Milicevic</td>
      <td>1.0</td>
      <td>casino|espionage|free running|james bond|terrorist</td>
      <td>http://www.imdb.com/title/tt0381061/?ref_=fn_tt_tt_1</td>
      <td>2301.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>150000000.0</td>
      <td>2006.0</td>
      <td>1000.0</td>
      <td>8.0</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4879</th>
      <td>Black and White</td>
      <td>Andrew Bujalski</td>
      <td>52.0</td>
      <td>109.0</td>
      <td>26.0</td>
      <td>3.0</td>
      <td>Kate Dollenmayer</td>
      <td>26.0</td>
      <td>NaN</td>
      <td>Comedy</td>
      <td>Andrew Bujalski</td>
      <td>Mutual Appreciation</td>
      <td>1578</td>
      <td>38</td>
      <td>Justin Rice</td>
      <td>0.0</td>
      <td>friendship|guitarist|mumblecore|musician|new york</td>
      <td>http://www.imdb.com/title/tt0446747/?ref_=fn_tt_tt_1</td>
      <td>23.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>NaN</td>
      <td>2005.0</td>
      <td>6.0</td>
      <td>6.9</td>
      <td>1.66</td>
      <td>91</td>
    </tr>
    <tr>
      <th>4882</th>
      <td>Black and White</td>
      <td>Kevin Smith</td>
      <td>136.0</td>
      <td>102.0</td>
      <td>0.0</td>
      <td>216.0</td>
      <td>Brian O'Halloran</td>
      <td>898.0</td>
      <td>3151130.0</td>
      <td>Comedy</td>
      <td>Jason Mewes</td>
      <td>Clerks</td>
      <td>181749</td>
      <td>2103</td>
      <td>Jeff Anderson</td>
      <td>4.0</td>
      <td>clerk|friend|hockey|video|video store</td>
      <td>http://www.imdb.com/title/tt0109445/?ref_=fn_tt_tt_1</td>
      <td>615.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>230000.0</td>
      <td>1994.0</td>
      <td>657.0</td>
      <td>7.8</td>
      <td>1.37</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4888</th>
      <td>Black and White</td>
      <td>Richard Linklater</td>
      <td>61.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Richard Linklater</td>
      <td>5.0</td>
      <td>1227508.0</td>
      <td>Comedy|Drama</td>
      <td>Tommy Pallotta</td>
      <td>Slacker</td>
      <td>15103</td>
      <td>5</td>
      <td>Jean Caffeine</td>
      <td>0.0</td>
      <td>austin texas|moon|pap smear|texas|twenty something</td>
      <td>http://www.imdb.com/title/tt0102943/?ref_=fn_tt_tt_1</td>
      <td>80.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>23000.0</td>
      <td>1991.0</td>
      <td>0.0</td>
      <td>7.1</td>
      <td>1.37</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>4895</th>
      <td>Black and White</td>
      <td>Jim Chuchu</td>
      <td>6.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>Olwenya Maina</td>
      <td>147.0</td>
      <td>NaN</td>
      <td>Drama</td>
      <td>Paul Ogola</td>
      <td>Stories of Our Lives</td>
      <td>70</td>
      <td>170</td>
      <td>Mugambi Nthiga</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt3973612/?ref_=fn_tt_tt_1</td>
      <td>1.0</td>
      <td>Swahili</td>
      <td>Kenya</td>
      <td>NaN</td>
      <td>15000.0</td>
      <td>2014.0</td>
      <td>19.0</td>
      <td>7.4</td>
      <td>NaN</td>
      <td>45</td>
    </tr>
    <tr>
      <th>4901</th>
      <td>Black and White</td>
      <td>Ivan Kavanagh</td>
      <td>12.0</td>
      <td>83.0</td>
      <td>18.0</td>
      <td>0.0</td>
      <td>Michael Parle</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>Horror</td>
      <td>Patrick O'Donnell</td>
      <td>Tin Can Man</td>
      <td>57</td>
      <td>15</td>
      <td>Emma Eliza Regan</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>http://www.imdb.com/title/tt1235811/?ref_=fn_tt_tt_1</td>
      <td>1.0</td>
      <td>English</td>
      <td>Ireland</td>
      <td>NaN</td>
      <td>10000.0</td>
      <td>2007.0</td>
      <td>5.0</td>
      <td>6.7</td>
      <td>1.33</td>
      <td>105</td>
    </tr>
  </tbody>
</table>
<p>204 rows × 28 columns</p>
</div>




```python
df[df['color'] == 'Color']["movie_title"]
```




    0                                         Avatar
    1       Pirates of the Caribbean: At World's End
    2                                        Spectre
    3                          The Dark Knight Rises
    5                                    John Carter
                              ...                   
    4911                     Signed Sealed Delivered
    4912                               The Following
    4913                        A Plague So Pleasant
    4914                            Shanghai Calling
    4915                           My Date with Drew
    Name: movie_title, Length: 4693, dtype: object




```python
df.loc[df['color']=='Black and White' , ["movie_title", "director_name"]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>movie_title</th>
      <th>director_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>111</th>
      <td>Pearl Harbor</td>
      <td>Michael Bay</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Die Another Day</td>
      <td>Lee Tamahori</td>
    </tr>
    <tr>
      <th>254</th>
      <td>The Aviator</td>
      <td>Martin Scorsese</td>
    </tr>
    <tr>
      <th>269</th>
      <td>Ali</td>
      <td>Michael Mann</td>
    </tr>
    <tr>
      <th>283</th>
      <td>Casino Royale</td>
      <td>Martin Campbell</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4879</th>
      <td>Mutual Appreciation</td>
      <td>Andrew Bujalski</td>
    </tr>
    <tr>
      <th>4882</th>
      <td>Clerks</td>
      <td>Kevin Smith</td>
    </tr>
    <tr>
      <th>4888</th>
      <td>Slacker</td>
      <td>Richard Linklater</td>
    </tr>
    <tr>
      <th>4895</th>
      <td>Stories of Our Lives</td>
      <td>Jim Chuchu</td>
    </tr>
    <tr>
      <th>4901</th>
      <td>Tin Can Man</td>
      <td>Ivan Kavanagh</td>
    </tr>
  </tbody>
</table>
<p>204 rows × 2 columns</p>
</div>




```python
grade[grade['국어'] >= 80]
grade.loc[grade['국어'] >= 80]
# grade.iloc[grade['국어'] >= 80] # iloc은 boolean indexing을 지원안함.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>75</td>
      <td>165</td>
      <td>82.5</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>60</td>
      <td>140</td>
      <td>70.0</td>
      <td>False</td>
      <td>불합격</td>
    </tr>
  </tbody>
</table>
</div>




```python
grade.loc[grade['국어'] >= 70, ['국어','평균', '총점']]
grade[grade['국어']>=70][['국어', '평균', '총점']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>평균</th>
      <th>총점</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>90.0</td>
      <td>180</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>85.0</td>
      <td>170</td>
    </tr>
    <tr>
      <th>id-3</th>
      <td>90</td>
      <td>82.5</td>
      <td>165</td>
    </tr>
    <tr>
      <th>id-4</th>
      <td>70</td>
      <td>75.0</td>
      <td>150</td>
    </tr>
    <tr>
      <th>id-5</th>
      <td>80</td>
      <td>70.0</td>
      <td>140</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 국어, 영어 모두 80점 이상
grade.loc[(grade['국어'] >= 80) & (grade['영어'] >= 80)]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>국어</th>
      <th>영어</th>
      <th>총점</th>
      <th>평균</th>
      <th>합격여부</th>
      <th>통과여부2</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id-1</th>
      <td>100</td>
      <td>80</td>
      <td>180</td>
      <td>90.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
    <tr>
      <th>id-2</th>
      <td>80</td>
      <td>90</td>
      <td>170</td>
      <td>85.0</td>
      <td>True</td>
      <td>합격</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>


```python
# movie dataframe의 index명을 컬럼 설정한다.
df = pd.read_csv('data/movie.csv')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Color</td>
      <td>Gore Verbinski</td>
      <td>302.0</td>
      <td>169.0</td>
      <td>563.0</td>
      <td>1000.0</td>
      <td>Orlando Bloom</td>
      <td>40000.0</td>
      <td>309404152.0</td>
      <td>Action|Adventure|Fantasy</td>
      <td>...</td>
      <td>1238.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>300000000.0</td>
      <td>2007.0</td>
      <td>5000.0</td>
      <td>7.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Color</td>
      <td>Sam Mendes</td>
      <td>602.0</td>
      <td>148.0</td>
      <td>0.0</td>
      <td>161.0</td>
      <td>Rory Kinnear</td>
      <td>11000.0</td>
      <td>200074175.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>994.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>245000000.0</td>
      <td>2015.0</td>
      <td>393.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>85000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Color</td>
      <td>Christopher Nolan</td>
      <td>813.0</td>
      <td>164.0</td>
      <td>22000.0</td>
      <td>23000.0</td>
      <td>Christian Bale</td>
      <td>27000.0</td>
      <td>448130642.0</td>
      <td>Action|Thriller</td>
      <td>...</td>
      <td>2701.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>250000000.0</td>
      <td>2012.0</td>
      <td>23000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>164000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>Doug Walker</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Rob Walker</td>
      <td>131.0</td>
      <td>NaN</td>
      <td>Documentary</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>7.1</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4911</th>
      <td>Color</td>
      <td>Scott Smith</td>
      <td>1.0</td>
      <td>87.0</td>
      <td>2.0</td>
      <td>318.0</td>
      <td>Daphne Zuniga</td>
      <td>637.0</td>
      <td>NaN</td>
      <td>Comedy|Drama</td>
      <td>...</td>
      <td>6.0</td>
      <td>English</td>
      <td>Canada</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2013.0</td>
      <td>470.0</td>
      <td>7.7</td>
      <td>NaN</td>
      <td>84</td>
    </tr>
    <tr>
      <th>4912</th>
      <td>Color</td>
      <td>NaN</td>
      <td>43.0</td>
      <td>43.0</td>
      <td>NaN</td>
      <td>319.0</td>
      <td>Valorie Curry</td>
      <td>841.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Mystery|Thriller</td>
      <td>...</td>
      <td>359.0</td>
      <td>English</td>
      <td>USA</td>
      <td>TV-14</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>593.0</td>
      <td>7.5</td>
      <td>16.00</td>
      <td>32000</td>
    </tr>
    <tr>
      <th>4913</th>
      <td>Color</td>
      <td>Benjamin Roberds</td>
      <td>13.0</td>
      <td>76.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Maxwell Moody</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Drama|Horror|Thriller</td>
      <td>...</td>
      <td>3.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>1400.0</td>
      <td>2013.0</td>
      <td>0.0</td>
      <td>6.3</td>
      <td>NaN</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4914</th>
      <td>Color</td>
      <td>Daniel Hsia</td>
      <td>14.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>489.0</td>
      <td>Daniel Henney</td>
      <td>946.0</td>
      <td>10443.0</td>
      <td>Comedy|Drama|Romance</td>
      <td>...</td>
      <td>9.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>NaN</td>
      <td>2012.0</td>
      <td>719.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>660</td>
    </tr>
    <tr>
      <th>4915</th>
      <td>Color</td>
      <td>Jon Gunn</td>
      <td>43.0</td>
      <td>90.0</td>
      <td>16.0</td>
      <td>16.0</td>
      <td>Brian Herzlinger</td>
      <td>86.0</td>
      <td>85222.0</td>
      <td>Documentary</td>
      <td>...</td>
      <td>84.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>1100.0</td>
      <td>2004.0</td>
      <td>23.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>456</td>
    </tr>
  </tbody>
</table>
<p>4916 rows × 28 columns</p>
</div>




```python
# 1.  상영시간 (duration)이 300 이상인 영화들 조회
df[df['duration'] >= 300]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Heaven's Gate</th>
      <td>Color</td>
      <td>Michael Cimino</td>
      <td>102.0</td>
      <td>325.0</td>
      <td>517.0</td>
      <td>678.0</td>
      <td>Sam Waterston</td>
      <td>12000.0</td>
      <td>1500000.0</td>
      <td>Adventure|Drama|Western</td>
      <td>...</td>
      <td>189.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>44000000.0</td>
      <td>1980.0</td>
      <td>849.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>Blood In, Blood Out</th>
      <td>Color</td>
      <td>Taylor Hackford</td>
      <td>12.0</td>
      <td>330.0</td>
      <td>138.0</td>
      <td>672.0</td>
      <td>Jesse Borrego</td>
      <td>848.0</td>
      <td>4496583.0</td>
      <td>Crime|Drama</td>
      <td>...</td>
      <td>129.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>35000000.0</td>
      <td>1993.0</td>
      <td>674.0</td>
      <td>8.0</td>
      <td>1.66</td>
      <td>6000</td>
    </tr>
    <tr>
      <th>Trapped</th>
      <td>Color</td>
      <td>NaN</td>
      <td>16.0</td>
      <td>511.0</td>
      <td>NaN</td>
      <td>51.0</td>
      <td>Ingvar Eggert Sigurðsson</td>
      <td>147.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Thriller</td>
      <td>...</td>
      <td>19.0</td>
      <td>Icelandic</td>
      <td>Iceland</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>63.0</td>
      <td>8.2</td>
      <td>16.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Carlos</th>
      <td>Color</td>
      <td>NaN</td>
      <td>108.0</td>
      <td>334.0</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>Nora von Waldstätten</td>
      <td>897.0</td>
      <td>145118.0</td>
      <td>Biography|Crime|Drama|Thriller</td>
      <td>...</td>
      <td>36.0</td>
      <td>English</td>
      <td>France</td>
      <td>Not Rated</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>7.7</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Legend of Suriyothai</th>
      <td>Color</td>
      <td>Chatrichalerm Yukol</td>
      <td>31.0</td>
      <td>300.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>Chatchai Plengpanich</td>
      <td>7.0</td>
      <td>454255.0</td>
      <td>Action|Adventure|Drama|History|War</td>
      <td>...</td>
      <td>47.0</td>
      <td>Thai</td>
      <td>Thailand</td>
      <td>R</td>
      <td>400000000.0</td>
      <td>2001.0</td>
      <td>6.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>124</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>




```python
# 2.  상영시간 (duration)이 300 이상인 영화들의 영화제목(movie_title)과 감독이름(director_name) 조회
df[df['duration'] >= 300]["director_name"]
```




    movie_title
    Heaven's Gate                    Michael Cimino
    Blood In, Blood Out             Taylor Hackford
    Trapped                                     NaN
    Carlos                                      NaN
    The Legend of Suriyothai    Chatrichalerm Yukol
    Name: director_name, dtype: object




```python
df.loc[df['duration']>=300, 'director_name']
```




    movie_title
    Heaven's Gate                    Michael Cimino
    Blood In, Blood Out             Taylor Hackford
    Trapped                                     NaN
    Carlos                                      NaN
    The Legend of Suriyothai    Chatrichalerm Yukol
    Name: director_name, dtype: object




```python
# 3.  감독이름(director_name)이 'Quentin Tarantino' 의 영화들만 조회
df.loc[df['director_name'] == 'Quentin Tarantino']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Django Unchained</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>765.0</td>
      <td>165.0</td>
      <td>16000.0</td>
      <td>265.0</td>
      <td>Christoph Waltz</td>
      <td>29000.0</td>
      <td>162804648.0</td>
      <td>Drama|Western</td>
      <td>...</td>
      <td>1193.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>100000000.0</td>
      <td>2012.0</td>
      <td>11000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>199000</td>
    </tr>
    <tr>
      <th>Inglourious Basterds</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>486.0</td>
      <td>153.0</td>
      <td>16000.0</td>
      <td>11000.0</td>
      <td>Brad Pitt</td>
      <td>13000.0</td>
      <td>120523073.0</td>
      <td>Adventure|Drama|War</td>
      <td>...</td>
      <td>1527.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>75000000.0</td>
      <td>2009.0</td>
      <td>11000.0</td>
      <td>8.3</td>
      <td>2.35</td>
      <td>42000</td>
    </tr>
    <tr>
      <th>The Hateful Eight</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>596.0</td>
      <td>187.0</td>
      <td>16000.0</td>
      <td>1000.0</td>
      <td>Jennifer Jason Leigh</td>
      <td>46000.0</td>
      <td>54116191.0</td>
      <td>Crime|Drama|Mystery|Thriller|Western</td>
      <td>...</td>
      <td>1018.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>44000000.0</td>
      <td>2015.0</td>
      <td>1000.0</td>
      <td>7.9</td>
      <td>2.76</td>
      <td>114000</td>
    </tr>
    <tr>
      <th>Kill Bill: Vol. 1</th>
      <td>Black and White</td>
      <td>Quentin Tarantino</td>
      <td>354.0</td>
      <td>111.0</td>
      <td>16000.0</td>
      <td>640.0</td>
      <td>Vivica A. Fox</td>
      <td>926.0</td>
      <td>70098138.0</td>
      <td>Action</td>
      <td>...</td>
      <td>2105.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>30000000.0</td>
      <td>2003.0</td>
      <td>890.0</td>
      <td>8.1</td>
      <td>2.35</td>
      <td>13000</td>
    </tr>
    <tr>
      <th>Kill Bill: Vol. 2</th>
      <td>Black and White</td>
      <td>Quentin Tarantino</td>
      <td>304.0</td>
      <td>137.0</td>
      <td>16000.0</td>
      <td>348.0</td>
      <td>Michael Parks</td>
      <td>890.0</td>
      <td>66207920.0</td>
      <td>Action|Crime|Drama|Thriller</td>
      <td>...</td>
      <td>935.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>30000000.0</td>
      <td>2004.0</td>
      <td>387.0</td>
      <td>8.0</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Jackie Brown</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>140.0</td>
      <td>154.0</td>
      <td>16000.0</td>
      <td>889.0</td>
      <td>Sid Haig</td>
      <td>22000.0</td>
      <td>39647595.0</td>
      <td>Crime|Thriller</td>
      <td>...</td>
      <td>462.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>12000000.0</td>
      <td>1997.0</td>
      <td>1000.0</td>
      <td>7.5</td>
      <td>1.85</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Pulp Fiction</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>215.0</td>
      <td>178.0</td>
      <td>16000.0</td>
      <td>857.0</td>
      <td>Eric Stoltz</td>
      <td>13000.0</td>
      <td>107930000.0</td>
      <td>Crime|Drama</td>
      <td>...</td>
      <td>2195.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>8000000.0</td>
      <td>1994.0</td>
      <td>902.0</td>
      <td>8.9</td>
      <td>2.35</td>
      <td>45000</td>
    </tr>
    <tr>
      <th>Reservoir Dogs</th>
      <td>Color</td>
      <td>Quentin Tarantino</td>
      <td>173.0</td>
      <td>99.0</td>
      <td>16000.0</td>
      <td>455.0</td>
      <td>Steve Buscemi</td>
      <td>16000.0</td>
      <td>2812029.0</td>
      <td>Crime|Drama|Thriller</td>
      <td>...</td>
      <td>931.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>1200000.0</td>
      <td>1992.0</td>
      <td>12000.0</td>
      <td>8.4</td>
      <td>2.35</td>
      <td>19000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 27 columns</p>
</div>




```python
# 4. James Cameron의 영화중 상영시간이 150분 이상인 영화들의 제목(movie_title), 상영시간(duration), 컬러여부(color) 조회
df.loc[(df['director_name'] == 'James Cameron') & (df['duration'] >= 150), 
       ['duration', 'color']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>duration</th>
      <th>color</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Avatar</th>
      <td>178.0</td>
      <td>Color</td>
    </tr>
    <tr>
      <th>Titanic</th>
      <td>194.0</td>
      <td>Color</td>
    </tr>
    <tr>
      <th>Terminator 2: Judgment Day</th>
      <td>153.0</td>
      <td>Color</td>
    </tr>
    <tr>
      <th>The Abyss</th>
      <td>171.0</td>
      <td>Color</td>
    </tr>
    <tr>
      <th>Aliens</th>
      <td>154.0</td>
      <td>Color</td>
    </tr>
  </tbody>
</table>
</div>



## query() 를 이용한 boolean indexing
- query(조회조건)
    - sql의 where 절의 조건 처럼 문자열의 query statement를 이용해 조건으로 조회
    - boolean index에 비해 
        - 장점: 편의성(문자열로 query statement를 만들므로 동적 구문 생성등 다양한 처리가 가능)과 가독성이 좋다.
        - 단점: 속도가 느리다.
- 조회조건 구문
    - `"컬럼명 연산자 비교값"`
- 외부변수를 이용해 query문의 비교값을 지정할 수 있다.
    - query 문자열 안에서 @변수명 사용
    - f string이나 format() 함수를 이용해 query를 만들 수도 있다.

### query 함수 연산자 
- **비교 연산자**
    - ==, \>, \>=, \<, \<=, !=
- **결측치 비교**
    - 컬럼.isna(), isnull()
    - 컬럼.notna(), notnull()       
- **논리 연산자**
    - and, or, not
- **in 연산자**
    - in, ==
    - not in, !=
    - 비교 대상값은 리스트에 넣는다.
- **Index name으로 검색**
    - 행의 index 이름으로 검색
- **문자열 부분검색(sql의 like)**
    - 컬럼명.str.contains(문자열): 문자열을 포함하고 있는 
    - 컬럼명.str.startswith(문자열): 문자열로 시작하는
    - 컬럼명.str.endswith(문자열): 문자열로 끝나는
    - **문자열 부분검색을 할 컬럼에 결측치(NaN)이 있으면 안된다.**


```python
import pandas as pd
import numpy as np
data_dict = {
    'name':['김영수', '박영희', '오준호', '조민경', '박영희', '김영수'],
    'age':[23, 17, 28, 31, 23, 17],
    'email':['kys@gmail.com', 'pyh@gmail.com', 'ojh@daum.net', 'cmk@naver.com', 'pyh@daum.net', np.nan]
}
df = pd.DataFrame(data_dict)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 비교연산
df.query("name == '김영수'")
df.query("age > 25")
df.query("name != '김영수'")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 결측치 비교
df.query("email.isnull()")
df.query("email.isna()")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.query("email.notnull()")  #결측치가 아닌것
df.query("email.notna()")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 논리연산
df.query("not age < 25")
df.query("~(age < 25)")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
  </tbody>
</table>
</div>




```python
# () 로 안묶어도 됨.
df.query("name == '박영희' and age > 20")
df.query("name == '박영희' & age > 20")   
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.query('name == "박영희" or  age > 25')
df.query('name == "박영희" |  age > 25')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
  </tbody>
</table>
</div>




```python
# in, not in 연산자, ==, !=
df.query('age in [17, 23]')
df.query('age == [17, 23]')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.query('age not in [17, 23]')
df.query('age != [17, 23]')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>오준호</td>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
  </tbody>
</table>
</div>




```python
# index 이름의 조건으로 비교 => 컬럼명자리에 "index"
df.query("index > 3")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.set_index("name").query("index == '김영수'")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>email</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>김영수</th>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>김영수</th>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.set_index("name").query("index > '박영희'")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>email</th>
    </tr>
    <tr>
      <th>name</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>오준호</th>
      <td>28</td>
      <td>ojh@daum.net</td>
    </tr>
    <tr>
      <th>조민경</th>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
  </tbody>
</table>
</div>




```python
## 문자열 부분조회 (sql의 like)
pd.__version__
```




    '2.1.0'




```python
np.__version__
```




    '1.25.2'




```python
# ~로 시작하는지?
df.query("name.str.startswith('김')")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ~로 끝나는지?
df.query('name.str.endswith("희")')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>박영희</td>
      <td>23</td>
      <td>pyh@daum.net</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ~를 포함하는지?
df.query('name.str.contains("민경")')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 부분일치 함수 사용하는 컬럼에는 결측치가 있으면 안된다.
df.query("email.str.endswith('.com')")
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\frame.py:4605, in DataFrame.query(self, expr, inplace, **kwargs)
       4604 try:
    -> 4605     result = self.loc[res]
       4606 except ValueError:
       4607     # when res is multi-dimensional loc raises, but this is sometimes a
       4608     # valid query
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\indexing.py:1153, in _LocationIndexer.__getitem__(self, key)
       1152 maybe_callable = com.apply_if_callable(key, self.obj)
    -> 1153 return self._getitem_axis(maybe_callable, axis=axis)
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\indexing.py:1374, in _LocIndexer._getitem_axis(self, key, axis)
       1373     return self._get_slice_axis(key, axis=axis)
    -> 1374 elif com.is_bool_indexer(key):
       1375     return self._getbool_axis(key, axis=axis)
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\common.py:133, in is_bool_indexer(key)
        130 if lib.is_bool_array(key_array, skipna=True):
        131     # Don't raise on e.g. ["A", "B", np.nan], see
        132     #  test_loc_getitem_list_of_labels_categoricalindex_with_na
    --> 133     raise ValueError(na_msg)
        134 return False
    

    ValueError: Cannot mask with non-boolean array containing NA / NaN values

    
    During handling of the above exception, another exception occurred:
    

    ValueError                                Traceback (most recent call last)

    Cell In[304], line 1
    ----> 1 df.query("email.str.endswith('.com')")
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\frame.py:4609, in DataFrame.query(self, expr, inplace, **kwargs)
       4605     result = self.loc[res]
       4606 except ValueError:
       4607     # when res is multi-dimensional loc raises, but this is sometimes a
       4608     # valid query
    -> 4609     result = self[res]
       4611 if inplace:
       4612     self._update_inplace(result)
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\frame.py:3886, in DataFrame.__getitem__(self, key)
       3883     return self.where(key)
       3885 # Do we have a (boolean) 1d indexer?
    -> 3886 if com.is_bool_indexer(key):
       3887     return self._getitem_bool_array(key)
       3889 # We are left with two options: a single key, and a collection of keys,
       3890 # We interpret tuples as collections only for non-MultiIndex
    

    File ~\anaconda3\envs\da\Lib\site-packages\pandas\core\common.py:133, in is_bool_indexer(key)
        129     na_msg = "Cannot mask with non-boolean array containing NA / NaN values"
        130     if lib.is_bool_array(key_array, skipna=True):
        131         # Don't raise on e.g. ["A", "B", np.nan], see
        132         #  test_loc_getitem_list_of_labels_categoricalindex_with_na
    --> 133         raise ValueError(na_msg)
        134     return False
        135 return True
    

    ValueError: Cannot mask with non-boolean array containing NA / NaN values



```python
# 결측치가 있을 경우 -> 결측치 아닌 것들을 조회하고 부분일치 조회.
a = df.query('email.notnull()').query("email.str.endswith('.com')")
a
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>1</th>
      <td>박영희</td>
      <td>17</td>
      <td>pyh@gmail.com</td>
    </tr>
    <tr>
      <th>3</th>
      <td>조민경</td>
      <td>31</td>
      <td>cmk@naver.com</td>
    </tr>
  </tbody>
</table>
</div>




```python
f'name.str.startswith("{first_name}")'
```




    'name.str.startswith("김")'




```python
# 조건 비교에서 사용할 값을 외부의 변수값을 이용하기
first_name = '김'
df.query(f'name.str.startswith("{first_name}")')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.query('name.str.startswith(@first_name)')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
    <tr>
      <th>5</th>
      <td>김영수</td>
      <td>17</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
name = input("이름:")
age = int(input("나이:"))
# 변수의값의 타입에 맞춰서 query문을 처리
df.query("name == @name and age == @age")  
```

    이름:김영수
    나이:23
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age</th>
      <th>email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>김영수</td>
      <td>23</td>
      <td>kys@gmail.com</td>
    </tr>
  </tbody>
</table>
</div>



<b style='font-size:2.2em'>TODO</b>
- movie dataframe을 이용해 query() 메소드를 사용해서 아래 문제를 푸세요. 


```python
import pandas as pd

df = pd.read_csv('data/movie.csv')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4916 entries, 0 to 4915
    Data columns (total 28 columns):
     #   Column                     Non-Null Count  Dtype  
    ---  ------                     --------------  -----  
     0   color                      4897 non-null   object 
     1   director_name              4814 non-null   object 
     2   num_critic_for_reviews     4867 non-null   float64
     3   duration                   4901 non-null   float64
     4   director_facebook_likes    4814 non-null   float64
     5   actor_3_facebook_likes     4893 non-null   float64
     6   actor_2_name               4903 non-null   object 
     7   actor_1_facebook_likes     4909 non-null   float64
     8   gross                      4054 non-null   float64
     9   genres                     4916 non-null   object 
     10  actor_1_name               4909 non-null   object 
     11  movie_title                4916 non-null   object 
     12  num_voted_users            4916 non-null   int64  
     13  cast_total_facebook_likes  4916 non-null   int64  
     14  actor_3_name               4893 non-null   object 
     15  facenumber_in_poster       4903 non-null   float64
     16  plot_keywords              4764 non-null   object 
     17  movie_imdb_link            4916 non-null   object 
     18  num_user_for_reviews       4895 non-null   float64
     19  language                   4902 non-null   object 
     20  country                    4911 non-null   object 
     21  content_rating             4616 non-null   object 
     22  budget                     4432 non-null   float64
     23  title_year                 4810 non-null   float64
     24  actor_2_facebook_likes     4903 non-null   float64
     25  imdb_score                 4916 non-null   float64
     26  aspect_ratio               4590 non-null   float64
     27  movie_facebook_likes       4916 non-null   int64  
    dtypes: float64(13), int64(3), object(12)
    memory usage: 1.1+ MB
    


```python
df.loc[df['duration'] >= 300]
```


```python
# 1. 상영시간이 300분 이상인 영화들만 조회
df.query("duration >= 300")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Heaven's Gate</th>
      <td>Color</td>
      <td>Michael Cimino</td>
      <td>102.0</td>
      <td>325.0</td>
      <td>517.0</td>
      <td>678.0</td>
      <td>Sam Waterston</td>
      <td>12000.0</td>
      <td>1500000.0</td>
      <td>Adventure|Drama|Western</td>
      <td>...</td>
      <td>189.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>44000000.0</td>
      <td>1980.0</td>
      <td>849.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>1000</td>
    </tr>
    <tr>
      <th>Blood In, Blood Out</th>
      <td>Color</td>
      <td>Taylor Hackford</td>
      <td>12.0</td>
      <td>330.0</td>
      <td>138.0</td>
      <td>672.0</td>
      <td>Jesse Borrego</td>
      <td>848.0</td>
      <td>4496583.0</td>
      <td>Crime|Drama</td>
      <td>...</td>
      <td>129.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>35000000.0</td>
      <td>1993.0</td>
      <td>674.0</td>
      <td>8.0</td>
      <td>1.66</td>
      <td>6000</td>
    </tr>
    <tr>
      <th>Trapped</th>
      <td>Color</td>
      <td>NaN</td>
      <td>16.0</td>
      <td>511.0</td>
      <td>NaN</td>
      <td>51.0</td>
      <td>Ingvar Eggert Sigurðsson</td>
      <td>147.0</td>
      <td>NaN</td>
      <td>Crime|Drama|Thriller</td>
      <td>...</td>
      <td>19.0</td>
      <td>Icelandic</td>
      <td>Iceland</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>63.0</td>
      <td>8.2</td>
      <td>16.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Carlos</th>
      <td>Color</td>
      <td>NaN</td>
      <td>108.0</td>
      <td>334.0</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>Nora von Waldstätten</td>
      <td>897.0</td>
      <td>145118.0</td>
      <td>Biography|Crime|Drama|Thriller</td>
      <td>...</td>
      <td>36.0</td>
      <td>English</td>
      <td>France</td>
      <td>Not Rated</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30.0</td>
      <td>7.7</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Legend of Suriyothai</th>
      <td>Color</td>
      <td>Chatrichalerm Yukol</td>
      <td>31.0</td>
      <td>300.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>Chatchai Plengpanich</td>
      <td>7.0</td>
      <td>454255.0</td>
      <td>Action|Adventure|Drama|History|War</td>
      <td>...</td>
      <td>47.0</td>
      <td>Thai</td>
      <td>Thailand</td>
      <td>R</td>
      <td>400000000.0</td>
      <td>2001.0</td>
      <td>6.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>124</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>




```python
# 2. 상영시간이 250분 ~ 300분 인 영화들 조회
df.query("duration >= 250 and duration <= 300")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gods and Generals</th>
      <td>Color</td>
      <td>Ron Maxwell</td>
      <td>84.0</td>
      <td>280.0</td>
      <td>33.0</td>
      <td>67.0</td>
      <td>Bruce Boxleitner</td>
      <td>789.0</td>
      <td>12870569.0</td>
      <td>Drama|History|War</td>
      <td>...</td>
      <td>497.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>56000000.0</td>
      <td>2003.0</td>
      <td>640.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>953</td>
    </tr>
    <tr>
      <th>Cleopatra</th>
      <td>Color</td>
      <td>Joseph L. Mankiewicz</td>
      <td>72.0</td>
      <td>251.0</td>
      <td>311.0</td>
      <td>595.0</td>
      <td>Richard Burton</td>
      <td>940.0</td>
      <td>57750000.0</td>
      <td>Biography|Drama|History|Romance</td>
      <td>...</td>
      <td>192.0</td>
      <td>English</td>
      <td>UK</td>
      <td>Approved</td>
      <td>31115000.0</td>
      <td>1963.0</td>
      <td>726.0</td>
      <td>7.0</td>
      <td>2.20</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Apocalypse Now</th>
      <td>Color</td>
      <td>Francis Ford Coppola</td>
      <td>261.0</td>
      <td>289.0</td>
      <td>0.0</td>
      <td>3000.0</td>
      <td>Marlon Brando</td>
      <td>11000.0</td>
      <td>78800000.0</td>
      <td>Drama|War</td>
      <td>...</td>
      <td>983.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>31500000.0</td>
      <td>1979.0</td>
      <td>10000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>19000</td>
    </tr>
    <tr>
      <th>Once Upon a Time in America</th>
      <td>Color</td>
      <td>Sergio Leone</td>
      <td>111.0</td>
      <td>251.0</td>
      <td>0.0</td>
      <td>642.0</td>
      <td>Burt Young</td>
      <td>22000.0</td>
      <td>5300000.0</td>
      <td>Crime|Drama</td>
      <td>...</td>
      <td>495.0</td>
      <td>English</td>
      <td>Italy</td>
      <td>R</td>
      <td>30000000.0</td>
      <td>1984.0</td>
      <td>683.0</td>
      <td>8.4</td>
      <td>1.85</td>
      <td>12000</td>
    </tr>
    <tr>
      <th>Gettysburg</th>
      <td>Color</td>
      <td>Ron Maxwell</td>
      <td>22.0</td>
      <td>271.0</td>
      <td>33.0</td>
      <td>251.0</td>
      <td>William Morgan Sheppard</td>
      <td>854.0</td>
      <td>10769960.0</td>
      <td>Drama|History|War</td>
      <td>...</td>
      <td>256.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>25000000.0</td>
      <td>1993.0</td>
      <td>702.0</td>
      <td>7.7</td>
      <td>1.85</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Arn: The Knight Templar</th>
      <td>Color</td>
      <td>Peter Flinth</td>
      <td>34.0</td>
      <td>270.0</td>
      <td>5.0</td>
      <td>292.0</td>
      <td>Michael Nyqvist</td>
      <td>908.0</td>
      <td>NaN</td>
      <td>Action|Adventure|Drama|Romance|War</td>
      <td>...</td>
      <td>54.0</td>
      <td>Swedish</td>
      <td>Sweden</td>
      <td>NaN</td>
      <td>25000000.0</td>
      <td>2007.0</td>
      <td>690.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Company</th>
      <td>Color</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>286.0</td>
      <td>NaN</td>
      <td>527.0</td>
      <td>Tom Hollander</td>
      <td>857.0</td>
      <td>NaN</td>
      <td>Drama|History|Thriller</td>
      <td>...</td>
      <td>39.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>555.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>733</td>
    </tr>
    <tr>
      <th>Das Boot</th>
      <td>Color</td>
      <td>Wolfgang Petersen</td>
      <td>96.0</td>
      <td>293.0</td>
      <td>249.0</td>
      <td>18.0</td>
      <td>Martin Semmelrogge</td>
      <td>362.0</td>
      <td>11433134.0</td>
      <td>Adventure|Drama|Thriller|War</td>
      <td>...</td>
      <td>426.0</td>
      <td>German</td>
      <td>West Germany</td>
      <td>R</td>
      <td>14000000.0</td>
      <td>1981.0</td>
      <td>21.0</td>
      <td>8.4</td>
      <td>1.85</td>
      <td>11000</td>
    </tr>
    <tr>
      <th>The Legend of Suriyothai</th>
      <td>Color</td>
      <td>Chatrichalerm Yukol</td>
      <td>31.0</td>
      <td>300.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>Chatchai Plengpanich</td>
      <td>7.0</td>
      <td>454255.0</td>
      <td>Action|Adventure|Drama|History|War</td>
      <td>...</td>
      <td>47.0</td>
      <td>Thai</td>
      <td>Thailand</td>
      <td>R</td>
      <td>400000000.0</td>
      <td>2001.0</td>
      <td>6.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>124</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 27 columns</p>
</div>




```python
df[df['duration'].between(250, 300)]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gods and Generals</th>
      <td>Color</td>
      <td>Ron Maxwell</td>
      <td>84.0</td>
      <td>280.0</td>
      <td>33.0</td>
      <td>67.0</td>
      <td>Bruce Boxleitner</td>
      <td>789.0</td>
      <td>12870569.0</td>
      <td>Drama|History|War</td>
      <td>...</td>
      <td>497.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>56000000.0</td>
      <td>2003.0</td>
      <td>640.0</td>
      <td>6.3</td>
      <td>2.35</td>
      <td>953</td>
    </tr>
    <tr>
      <th>Cleopatra</th>
      <td>Color</td>
      <td>Joseph L. Mankiewicz</td>
      <td>72.0</td>
      <td>251.0</td>
      <td>311.0</td>
      <td>595.0</td>
      <td>Richard Burton</td>
      <td>940.0</td>
      <td>57750000.0</td>
      <td>Biography|Drama|History|Romance</td>
      <td>...</td>
      <td>192.0</td>
      <td>English</td>
      <td>UK</td>
      <td>Approved</td>
      <td>31115000.0</td>
      <td>1963.0</td>
      <td>726.0</td>
      <td>7.0</td>
      <td>2.20</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Apocalypse Now</th>
      <td>Color</td>
      <td>Francis Ford Coppola</td>
      <td>261.0</td>
      <td>289.0</td>
      <td>0.0</td>
      <td>3000.0</td>
      <td>Marlon Brando</td>
      <td>11000.0</td>
      <td>78800000.0</td>
      <td>Drama|War</td>
      <td>...</td>
      <td>983.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>31500000.0</td>
      <td>1979.0</td>
      <td>10000.0</td>
      <td>8.5</td>
      <td>2.35</td>
      <td>19000</td>
    </tr>
    <tr>
      <th>Once Upon a Time in America</th>
      <td>Color</td>
      <td>Sergio Leone</td>
      <td>111.0</td>
      <td>251.0</td>
      <td>0.0</td>
      <td>642.0</td>
      <td>Burt Young</td>
      <td>22000.0</td>
      <td>5300000.0</td>
      <td>Crime|Drama</td>
      <td>...</td>
      <td>495.0</td>
      <td>English</td>
      <td>Italy</td>
      <td>R</td>
      <td>30000000.0</td>
      <td>1984.0</td>
      <td>683.0</td>
      <td>8.4</td>
      <td>1.85</td>
      <td>12000</td>
    </tr>
    <tr>
      <th>Gettysburg</th>
      <td>Color</td>
      <td>Ron Maxwell</td>
      <td>22.0</td>
      <td>271.0</td>
      <td>33.0</td>
      <td>251.0</td>
      <td>William Morgan Sheppard</td>
      <td>854.0</td>
      <td>10769960.0</td>
      <td>Drama|History|War</td>
      <td>...</td>
      <td>256.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>25000000.0</td>
      <td>1993.0</td>
      <td>702.0</td>
      <td>7.7</td>
      <td>1.85</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Arn: The Knight Templar</th>
      <td>Color</td>
      <td>Peter Flinth</td>
      <td>34.0</td>
      <td>270.0</td>
      <td>5.0</td>
      <td>292.0</td>
      <td>Michael Nyqvist</td>
      <td>908.0</td>
      <td>NaN</td>
      <td>Action|Adventure|Drama|Romance|War</td>
      <td>...</td>
      <td>54.0</td>
      <td>Swedish</td>
      <td>Sweden</td>
      <td>NaN</td>
      <td>25000000.0</td>
      <td>2007.0</td>
      <td>690.0</td>
      <td>6.6</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Company</th>
      <td>Color</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>286.0</td>
      <td>NaN</td>
      <td>527.0</td>
      <td>Tom Hollander</td>
      <td>857.0</td>
      <td>NaN</td>
      <td>Drama|History|Thriller</td>
      <td>...</td>
      <td>39.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>555.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>733</td>
    </tr>
    <tr>
      <th>Das Boot</th>
      <td>Color</td>
      <td>Wolfgang Petersen</td>
      <td>96.0</td>
      <td>293.0</td>
      <td>249.0</td>
      <td>18.0</td>
      <td>Martin Semmelrogge</td>
      <td>362.0</td>
      <td>11433134.0</td>
      <td>Adventure|Drama|Thriller|War</td>
      <td>...</td>
      <td>426.0</td>
      <td>German</td>
      <td>West Germany</td>
      <td>R</td>
      <td>14000000.0</td>
      <td>1981.0</td>
      <td>21.0</td>
      <td>8.4</td>
      <td>1.85</td>
      <td>11000</td>
    </tr>
    <tr>
      <th>The Legend of Suriyothai</th>
      <td>Color</td>
      <td>Chatrichalerm Yukol</td>
      <td>31.0</td>
      <td>300.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>Chatchai Plengpanich</td>
      <td>7.0</td>
      <td>454255.0</td>
      <td>Action|Adventure|Drama|History|War</td>
      <td>...</td>
      <td>47.0</td>
      <td>Thai</td>
      <td>Thailand</td>
      <td>R</td>
      <td>400000000.0</td>
      <td>2001.0</td>
      <td>6.0</td>
      <td>6.6</td>
      <td>1.85</td>
      <td>124</td>
    </tr>
  </tbody>
</table>
<p>9 rows × 27 columns</p>
</div>




```python
# 3. 컬러영화가 아닌 영화 조회
df.query("color != 'Color'") # color가 아닌값들의 행  + 결측치행
df.query('color != "Color" and color.notnull()')  # 결측치 빼고 조회.
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Pearl Harbor</th>
      <td>Black and White</td>
      <td>Michael Bay</td>
      <td>191.0</td>
      <td>184.0</td>
      <td>0.0</td>
      <td>691.0</td>
      <td>Jaime King</td>
      <td>3000.0</td>
      <td>198539855.0</td>
      <td>Action|Drama|History|Romance|War</td>
      <td>...</td>
      <td>1999.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>140000000.0</td>
      <td>2001.0</td>
      <td>961.0</td>
      <td>6.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Die Another Day</th>
      <td>Black and White</td>
      <td>Lee Tamahori</td>
      <td>264.0</td>
      <td>133.0</td>
      <td>93.0</td>
      <td>746.0</td>
      <td>Colin Salmon</td>
      <td>769.0</td>
      <td>160201106.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>1185.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>142000000.0</td>
      <td>2002.0</td>
      <td>766.0</td>
      <td>6.1</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>The Aviator</th>
      <td>Black and White</td>
      <td>Martin Scorsese</td>
      <td>267.0</td>
      <td>170.0</td>
      <td>17000.0</td>
      <td>827.0</td>
      <td>Adam Scott</td>
      <td>29000.0</td>
      <td>102608827.0</td>
      <td>Biography|Drama</td>
      <td>...</td>
      <td>799.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>110000000.0</td>
      <td>2004.0</td>
      <td>3000.0</td>
      <td>7.5</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Ali</th>
      <td>Black and White</td>
      <td>Michael Mann</td>
      <td>174.0</td>
      <td>165.0</td>
      <td>0.0</td>
      <td>780.0</td>
      <td>Jada Pinkett Smith</td>
      <td>10000.0</td>
      <td>58183966.0</td>
      <td>Biography|Drama|Sport</td>
      <td>...</td>
      <td>386.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>107000000.0</td>
      <td>2001.0</td>
      <td>851.0</td>
      <td>6.8</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Casino Royale</th>
      <td>Black and White</td>
      <td>Martin Campbell</td>
      <td>400.0</td>
      <td>144.0</td>
      <td>258.0</td>
      <td>834.0</td>
      <td>Tobias Menzies</td>
      <td>6000.0</td>
      <td>167007184.0</td>
      <td>Action|Adventure|Thriller</td>
      <td>...</td>
      <td>2301.0</td>
      <td>English</td>
      <td>UK</td>
      <td>PG-13</td>
      <td>150000000.0</td>
      <td>2006.0</td>
      <td>1000.0</td>
      <td>8.0</td>
      <td>2.35</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Mutual Appreciation</th>
      <td>Black and White</td>
      <td>Andrew Bujalski</td>
      <td>52.0</td>
      <td>109.0</td>
      <td>26.0</td>
      <td>3.0</td>
      <td>Kate Dollenmayer</td>
      <td>26.0</td>
      <td>NaN</td>
      <td>Comedy</td>
      <td>...</td>
      <td>23.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>NaN</td>
      <td>2005.0</td>
      <td>6.0</td>
      <td>6.9</td>
      <td>1.66</td>
      <td>91</td>
    </tr>
    <tr>
      <th>Clerks</th>
      <td>Black and White</td>
      <td>Kevin Smith</td>
      <td>136.0</td>
      <td>102.0</td>
      <td>0.0</td>
      <td>216.0</td>
      <td>Brian O'Halloran</td>
      <td>898.0</td>
      <td>3151130.0</td>
      <td>Comedy</td>
      <td>...</td>
      <td>615.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>230000.0</td>
      <td>1994.0</td>
      <td>657.0</td>
      <td>7.8</td>
      <td>1.37</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Slacker</th>
      <td>Black and White</td>
      <td>Richard Linklater</td>
      <td>61.0</td>
      <td>100.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Richard Linklater</td>
      <td>5.0</td>
      <td>1227508.0</td>
      <td>Comedy|Drama</td>
      <td>...</td>
      <td>80.0</td>
      <td>English</td>
      <td>USA</td>
      <td>R</td>
      <td>23000.0</td>
      <td>1991.0</td>
      <td>0.0</td>
      <td>7.1</td>
      <td>1.37</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>Stories of Our Lives</th>
      <td>Black and White</td>
      <td>Jim Chuchu</td>
      <td>6.0</td>
      <td>60.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>Olwenya Maina</td>
      <td>147.0</td>
      <td>NaN</td>
      <td>Drama</td>
      <td>...</td>
      <td>1.0</td>
      <td>Swahili</td>
      <td>Kenya</td>
      <td>NaN</td>
      <td>15000.0</td>
      <td>2014.0</td>
      <td>19.0</td>
      <td>7.4</td>
      <td>NaN</td>
      <td>45</td>
    </tr>
    <tr>
      <th>Tin Can Man</th>
      <td>Black and White</td>
      <td>Ivan Kavanagh</td>
      <td>12.0</td>
      <td>83.0</td>
      <td>18.0</td>
      <td>0.0</td>
      <td>Michael Parle</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>Horror</td>
      <td>...</td>
      <td>1.0</td>
      <td>English</td>
      <td>Ireland</td>
      <td>NaN</td>
      <td>10000.0</td>
      <td>2007.0</td>
      <td>5.0</td>
      <td>6.7</td>
      <td>1.33</td>
      <td>105</td>
    </tr>
  </tbody>
</table>
<p>204 rows × 27 columns</p>
</div>




```python
# 4. 이름에 James가 들어가는 감독의 영화조회
df.query("director_name.notnull()").query("director_name.str.contains('James')")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>color</th>
      <th>director_name</th>
      <th>num_critic_for_reviews</th>
      <th>duration</th>
      <th>director_facebook_likes</th>
      <th>actor_3_facebook_likes</th>
      <th>actor_2_name</th>
      <th>actor_1_facebook_likes</th>
      <th>gross</th>
      <th>genres</th>
      <th>...</th>
      <th>num_user_for_reviews</th>
      <th>language</th>
      <th>country</th>
      <th>content_rating</th>
      <th>budget</th>
      <th>title_year</th>
      <th>actor_2_facebook_likes</th>
      <th>imdb_score</th>
      <th>aspect_ratio</th>
      <th>movie_facebook_likes</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Avatar</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>723.0</td>
      <td>178.0</td>
      <td>0.0</td>
      <td>855.0</td>
      <td>Joel David Moore</td>
      <td>1000.0</td>
      <td>760505847.0</td>
      <td>Action|Adventure|Fantasy|Sci-Fi</td>
      <td>...</td>
      <td>3054.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>237000000.0</td>
      <td>2009.0</td>
      <td>936.0</td>
      <td>7.9</td>
      <td>1.78</td>
      <td>33000</td>
    </tr>
    <tr>
      <th>Titanic</th>
      <td>Color</td>
      <td>James Cameron</td>
      <td>315.0</td>
      <td>194.0</td>
      <td>0.0</td>
      <td>794.0</td>
      <td>Kate Winslet</td>
      <td>29000.0</td>
      <td>658672302.0</td>
      <td>Drama|Romance</td>
      <td>...</td>
      <td>2528.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>200000000.0</td>
      <td>1997.0</td>
      <td>14000.0</td>
      <td>7.7</td>
      <td>2.35</td>
      <td>26000</td>
    </tr>
    <tr>
      <th>Furious 7</th>
      <td>Color</td>
      <td>James Wan</td>
      <td>424.0</td>
      <td>140.0</td>
      <td>0.0</td>
      <td>14000.0</td>
      <td>Paul Walker</td>
      <td>26000.0</td>
      <td>350034110.0</td>
      <td>Action|Crime|Thriller</td>
      <td>...</td>
      <td>657.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>190000000.0</td>
      <td>2015.0</td>
      <td>23000.0</td>
      <td>7.2</td>
      <td>2.35</td>
      <td>94000</td>
    </tr>
    <tr>
      <th>Guardians of the Galaxy</th>
      <td>Color</td>
      <td>James Gunn</td>
      <td>653.0</td>
      <td>121.0</td>
      <td>571.0</td>
      <td>3000.0</td>
      <td>Vin Diesel</td>
      <td>14000.0</td>
      <td>333130696.0</td>
      <td>Action|Adventure|Sci-Fi</td>
      <td>...</td>
      <td>1097.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>170000000.0</td>
      <td>2014.0</td>
      <td>14000.0</td>
      <td>8.1</td>
      <td>2.35</td>
      <td>96000</td>
    </tr>
    <tr>
      <th>Alice Through the Looking Glass</th>
      <td>Color</td>
      <td>James Bobin</td>
      <td>218.0</td>
      <td>113.0</td>
      <td>33.0</td>
      <td>11000.0</td>
      <td>Alan Rickman</td>
      <td>40000.0</td>
      <td>76846624.0</td>
      <td>Adventure|Family|Fantasy</td>
      <td>...</td>
      <td>131.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>170000000.0</td>
      <td>2016.0</td>
      <td>25000.0</td>
      <td>6.4</td>
      <td>1.85</td>
      <td>30000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>Bambi</th>
      <td>Color</td>
      <td>James Algar</td>
      <td>116.0</td>
      <td>70.0</td>
      <td>11.0</td>
      <td>8.0</td>
      <td>Donnie Dunagan</td>
      <td>16.0</td>
      <td>102797150.0</td>
      <td>Animation|Drama|Family</td>
      <td>...</td>
      <td>136.0</td>
      <td>English</td>
      <td>USA</td>
      <td>Approved</td>
      <td>NaN</td>
      <td>1942.0</td>
      <td>12.0</td>
      <td>7.4</td>
      <td>1.33</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Hoop Dreams</th>
      <td>Color</td>
      <td>Steve James</td>
      <td>53.0</td>
      <td>170.0</td>
      <td>23.0</td>
      <td>2.0</td>
      <td>Arthur Agee</td>
      <td>7.0</td>
      <td>7830611.0</td>
      <td>Documentary|Drama|Sport</td>
      <td>...</td>
      <td>74.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG-13</td>
      <td>700000.0</td>
      <td>1994.0</td>
      <td>6.0</td>
      <td>8.3</td>
      <td>1.33</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Western Religion</th>
      <td>Color</td>
      <td>James O'Brien</td>
      <td>5.0</td>
      <td>105.0</td>
      <td>7.0</td>
      <td>494.0</td>
      <td>Vivian Lamolli</td>
      <td>814.0</td>
      <td>NaN</td>
      <td>Adventure|Drama|Fantasy|Thriller|Western</td>
      <td>...</td>
      <td>4.0</td>
      <td>English</td>
      <td>USA</td>
      <td>NaN</td>
      <td>250000.0</td>
      <td>2015.0</td>
      <td>755.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>244</td>
    </tr>
    <tr>
      <th>Yesterday Was a Lie</th>
      <td>Black and White</td>
      <td>James Kerwin</td>
      <td>25.0</td>
      <td>89.0</td>
      <td>0.0</td>
      <td>31.0</td>
      <td>Chase Masterson</td>
      <td>236.0</td>
      <td>NaN</td>
      <td>Drama|Music|Mystery|Romance|Sci-Fi</td>
      <td>...</td>
      <td>10.0</td>
      <td>English</td>
      <td>USA</td>
      <td>PG</td>
      <td>2500000.0</td>
      <td>2008.0</td>
      <td>189.0</td>
      <td>5.4</td>
      <td>1.78</td>
      <td>83</td>
    </tr>
    <tr>
      <th>Pink Narcissus</th>
      <td>Color</td>
      <td>James Bidgood</td>
      <td>8.0</td>
      <td>65.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>Bobby Kendall</td>
      <td>0.0</td>
      <td>8231.0</td>
      <td>Drama|Fantasy</td>
      <td>...</td>
      <td>16.0</td>
      <td>English</td>
      <td>USA</td>
      <td>Not Rated</td>
      <td>27000.0</td>
      <td>1971.0</td>
      <td>0.0</td>
      <td>6.7</td>
      <td>1.37</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
<p>89 rows × 27 columns</p>
</div>




```python
df.query("duration > 300")[['director_name', 'duration']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>director_name</th>
      <th>duration</th>
    </tr>
    <tr>
      <th>movie_title</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Heaven's Gate</th>
      <td>Michael Cimino</td>
      <td>325.0</td>
    </tr>
    <tr>
      <th>Blood In, Blood Out</th>
      <td>Taylor Hackford</td>
      <td>330.0</td>
    </tr>
    <tr>
      <th>Trapped</th>
      <td>NaN</td>
      <td>511.0</td>
    </tr>
    <tr>
      <th>Carlos</th>
      <td>NaN</td>
      <td>334.0</td>
    </tr>
  </tbody>
</table>
</div>


