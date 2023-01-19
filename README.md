# pandas_data_storage_compare
Pandas 라이브러리 내의 데이터 저장 형식을 비교한 자료입니다.

비교를 위해 아래 두 개의 라이브러리 설치가 필요합니다.
```
pip install pyarrow
```
```
pip install fastparquet
```

데이터 분석을 하다보면 Pandas를 이용하여 데이터를 읽고 저장합니다.
이 때, 다른 이와 공유하기 쉽고 보편적인 이유로 CSV를 파일 형식으로 주로 선택합니다.

CSV외에도 데이터를 저장하는 방식인 pickle, parquet, feather을 사용해보고 그 속도와 용량을 체크해봅니다.

## Pandas vs Pickle vs Parquet vs Feather
### Create DataSet
```
def get_dataset(size):
    """Create Fake Dataset: Food, Color, Age, Date, Bool, Prob
    Args:
        size (int): Size of Fake Dataset

    Returns:
        df (pd.DataFrame): Fake Dataset
    """
    df = pd.DataFrame()
    df['Food'] = np.random.choice(['Chicken','Pizza','Bread', 'Cheese', 'Meat'], size)
    df['Color'] = np.random.choice(['Red','Blue','Yellow','Green', 'Black'], size)
    df['Age'] = np.random.randint(1, 99, size)
    dates = pd.date_range('2022-01-01', '2099-12-31')
    df['date'] = np.random.choice(dates, size)
    df['Bool'] = np.random.choice(['yes','no'], size)
    df['Prob'] = np.random.uniform(0, 1, size)
    return df
df = get_dataset(5_000_000)
```
### Read&Write Time 비교
![image](https://user-images.githubusercontent.com/80809187/213463265-04e2b9b6-c590-49ce-9a63-871e980968fb.png)

### File Size 비교
![image](https://user-images.githubusercontent.com/80809187/213463398-92765f17-0d58-4b34-804a-1ebefbd5d6c8.png)

### DataType
Pandas는 아래와 같이 칼럼 별 데이터 타입을 저장하지 못합니다.
<img width="476" alt="image" src="https://user-images.githubusercontent.com/80809187/213463556-1bf595e6-c15c-4eab-a624-234c208b5ab0.png">

pickle, parquet, feather는 아래와 같이 칼럼 별 데이터 타입을 저장하였습니다.
<img width="369" alt="image" src="https://user-images.githubusercontent.com/80809187/213464071-76f179b0-0017-4ab6-8f7c-5f0000c1d2c9.png">

<img width="369" alt="image" src="https://user-images.githubusercontent.com/80809187/213463970-08f94c05-b60e-4b98-886c-54b3a8d6a905.png">

<img width="369" alt="image" src="https://user-images.githubusercontent.com/80809187/213464139-29da5e8f-c258-47df-8f88-abb86f09ccd3.png">

#### Read& Write Time 비교
> 데이터 타입을 변경한 후 Read, Write 시간을 비교하였습니다.
<img width="556" alt="image" src="https://user-images.githubusercontent.com/80809187/213464432-d0b64f0c-a877-4d21-910f-49b3c9505b61.png">

#### File Size 비교
> 데이터 타입을 변경한 후 File Size을 비교하였습니다.
<img width="549" alt="image" src="https://user-images.githubusercontent.com/80809187/213464689-60282923-9690-4756-854a-035653204b6d.png">
