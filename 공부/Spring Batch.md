# 😀 Spring Batch

 
### 대용량의 데이터를 효율적으로 처리
- 다중 스레드, 병렬 스텝 처리 등 효율적인 처리 가능

### 다양한 상황에 따른 대처 가능
- skip, retry 등 다양한 상황에 따른 대처를 할 수 있음
- 다양한 listener를 가지고 있으므로 작업 전, 후 에러 발생시 대처 가능

### 해당 로직의 결과를 직관적 제공
- 기본적으로 제공하는 Spring Batch 테이블을 통해 작업의 실행, 완료 시간, 성공, 실패 확인 가능
  

## 🍕 Job

- Job은 배치처리 과정을 하나의 단위로 만들어 놓은 객체입니다. 여러 Step 인스턴스를 포함하는 컨테이너, 배치처리 과정에 있어 전체 계층 최상단에 위치하고 있습니다. 

### JobInstance 
- Job의 실행의 단위
- Job을 실행시키게 되면 하나의 JobInstance가 생성됩니다.
- 1월 1일에 실행한 JobInstance가 실패하여 다시 실행 시키더라고 1월 1일에 대한 데이터만 처리함
  
### JobParameters
- JobInstance를 JobParameters 객체로 구분하게 됩니다.
- String, Double, Long, Date 4가지 형식만을 지원하고 있습니다.
  
### JobExecution
- JobInstance 실행에 시도에 대한 객체
- JobInstance 실행마다 별도의 JobExecution이 생성된다.
- JobExecution은 JobInstance 실행에 대한 상태, 시작시간, 종료시간, 생성시간 등의 정볼르 담고 있습니다.  
  
### JobRepository
- 모든 배치 처리 정보를 담고있는 매커니즘입니다.
- Job이 실행되게 되면 JobRepository에 JobExecution과 StepExecution을 생성하게 되며 JobRepository에서 Execution 정보들을 저장하고 조회하며 사용하게 됩니다.
  
## 🌭 Step

- Step은 Job의 배치처리를 정의하고 순차적인 단계를 캡슐화 합니다. Job은 최소한 1개 이상의 Step을 가져야 하며 Job의 실제 일괄 처리를 제어하는 모든 정보가 들어있습니다.

### StepExecution
- JobExecution과 동일하게 Step 실행 시도에 대한 객체를 나타냅니다.
- 이전 단계의 Step  이 실패하게 되면 다음 단계가 실행되지 않으므로 에러 이후의 StepExceution은 생성되지 않습니다.
- 실제 시작이 될때만 생성됩니다.
- JobExecution에 저장되는 정보 외에 read 수, write 수, commit 수, skip 수 등의 정보들도 저장이 됩니다.
  
### ExecutionContext
- Job에서 데이터를 공유할 수 있는 데이터 저장소입니다.
- ExecutionContext는 JobExecutionContext, StepExecutionContext 2가지 종류가 있으나 이 두가지는 지정되는 범위가 다릅니다. 
- JobExecutionContext의 경우 Commit 시점에 저장
- StepExecutionContext는 실행 사이에 저장이 되게 됩니다.
- ExecutionContext를 통해 Step간 Data 공유가 가능하며 Job 실패시 ExecutionContext를 통한 마지막 실행 값을 재구성 할 수 있습니다.

![image](https://user-images.githubusercontent.com/71022555/179394027-029bd0ed-735f-47f3-b1f5-113e913812e9.png)  
  
## 🧂 Item

### ItemReader
- Step에서 Item을 읽어오는 인터페이스입니다. ItemReader에 대한 다양한 인터페이스가 존재하며 다양한 방법으로 Item을 읽어 올 수 있습니다.  

### ItemProcessor
- ItemReader에서 읽어온 Item(데이터)를 처리하는 역할
- Reader, Writer는 필수지만 Processor는 필수가 아니다.
  
### ItemWriter
- 처리된 Data를 Writer 할 때 사용한다.

![image](https://user-images.githubusercontent.com/71022555/181202071-e4f8c371-69a7-4a0a-b38d-751f7ef73800.png)  
  

## 📃 파일 입력

## 🎈 플랫 파일(Flat File)
- 한 개 또는 그 이상의 레코드가 포함된 특정 파일
- 파일 내용을 봐도 데이터의 의미를 알 수 없다는 점에서 XML 파일과 차이가 있다.
- 플랫 파일에는 파일 내에 데이터의 포맷이나 의미를 정의하는 메타데이터가 없다.
- XML 파일은 태그를 사용해 데이터의 의미를 부여한다.
- 플랫 파일을 처리하는 ItemReader를 알아보기 전에 스프링 배치에서 파일을 읽을 때 사용하는 컴포넌트
- org.sprignframeowrk.batch.item.file.FlatFileItemReader는 메인 컴포넌트 두 개로 이뤄진다.
  - 읽어들일 대상 파일을 나타내는 스프링의 Resource
  - org.springframewrok.batch.item.file.LineMapper 인터페이스 구현체이다.
   
### LineMapper
- JDBC에서 RowMapper가 담당하는 것과 비슷한 역할을 한다.
- 스프링 JDBC에서 RowMapper를 사용하면 필드의 묶을을 나타내는 ResultSet을 객체로 매핑할 수 있다.
- 파일을 읽을 때 파일에서 레코드 한 개에 해당하는 문자열이 LineMapper 구현체에 전달된다.
- 가장 많이 사용하는 LineMapper 구현체는 DefaultLineMapper이다.
  - DefaultLineMapper는 파일에서 읽은 원시(raw) String을 대상으로 두 단계 처리(LineTokenizer, FieldSetMapper)를 거쳐 이후 처리에서 사용할 도메인 객체로 변환한다.

### LinkTokenizer 
- LinkTokenizer 구현체가 해당 줄을 파싱해 org.springframework.batch.item.file.FieldSet으로 만든다.
- LineTokenizer에 제공되는 String은 파일에서 가져온 한 줄 전체를 나타낸다.
- 레코드 내의 각 필드를 도메인 객체로 매핑하려면 해당 줄을 파싱해 각 필드를 나타내는 데이터의 모음으로 변환할 수 있어야 한다.
- Spring Batch의 FieldSet은 한 줄에 해당하는 필드의 모음을 나타낸다. 이는 데이터베이스로 작업할 때 사용하는 java.sql.ResultSet과 유사하다.

### FileSetMapper
- FileSetMapper 구현체는 FileSet을 도메인 객체로 매핑한다.
- LineTokenizer가 한 줄을 여러 필드로 나눴으므로, 이제 각 입력 필드를 도메인 객체의 필드로 매핑할 수 있다.
- Spring JDBC에서 RowMapper가 ResultSet의 로우(row)를 도메인 객체로 매핑하는 것과 흡사하다.

```java
@Component
@RequiredArgsConstructor
public class CsvReader {

    @Value("${csv.file.path}")
    private String fileLink;
    public FlatFileItemReader<BanNumberDto> csvFileItemReader() {
        FlatFileItemReader<BanNumberDto> flatFileItemReader = new FlatFileItemReader<>();
        flatFileItemReader.setResource(new FileSystemResource(fileLink));
        flatFileItemReader.setLinesToSkip(1); // header skip
        flatFileItemReader.setEncoding("UTF-8");

        DefaultLineMapper<BanNumberDto> defaultLineMapper = new DefaultLineMapper<>();
        /* delimitedLineTokenizer : setNames를 통해 각각의 데이터의 이름 설정 */

        DelimitedLineTokenizer delimitedLineTokenizer = new DelimitedLineTokenizer(",");
        delimitedLineTokenizer.setNames("kind", "number", "use");
        defaultLineMapper.setLineTokenizer(delimitedLineTokenizer);
        
        /* beanWrapperFieldSetMapper : Tokenizer에서 가지고온 데이터들을 VO로 바인드하는 역할 */
        BeanWrapperFieldSetMapper<BanNumberDto> beanWrapperFieldSetMapper = new BeanWrapperFieldSetMapper<>();
        beanWrapperFieldSetMapper.setTargetType(BanNumberDto.class);

        defaultLineMapper.setFieldSetMapper(beanWrapperFieldSetMapper);

        // LineMapper 지정
        flatFileItemReader.setLineMapper(defaultLineMapper);

        return flatFileItemReader;
    }
}
```


## 📜 파일 출력

```java
@Component
@RequiredArgsConstructor
public class CsvWriter implements ItemWriter<BanNumberDto> {
    private final BanNumberRepository banNumberRepository;

    private final BanNumberMapper banNumberMapper;
    @Override
    @Transactional
    public void write(List<? extends BanNumberDto> items){
        for(BanNumberDto item : items){
            banNumberRepository.save(banNumberMapper.toBanNumber(item));
        }
    }
}
```

## 🍟 에러 처리
```java
@Bean
public Step csvFileItemReaderStep() {
    return stepBuilderFactory.get("csvFileItemReaderStep")
            .<BanNumberDto, BanNumber>chunk(CHUNK_SIZE)
            .reader(csvReader.csvFileItemReader())
            .processor(csvConvert)
            .writer(csvWriter)
            .listener(new CsvErrorListener()) // 에러 핸들러
            .build();
}

// 1. Annotation 사용해서 에러 핸들링
@Slf4j
public class CsvErrorListener {
    @OnReadError
    public void onReaderError(Exception e){
        if(e instanceof FlatFileParseException) {
            FlatFileParseException ffpe = (FlatFileParseException) e;

            StringBuilder errorMessage = new StringBuilder();
            errorMessage.append("An error occured while processing the " +
                    ffpe.getLineNumber() +
                    " line of the file.  Below was the faulty " +
                    "input.\n");
            errorMessage.append(ffpe.getInput() + "\n");
            log.error(errorMessage.toString(), ffpe);
        } else {
            log.error("An error occured in ##csvReader##", e);
        }
    }

    @OnProcessError
    public void onProcessError(BanNumberDto banNumberDto, Exception e){
        log.error("An error occured in ##csvProcess##", e);
    }

    @OnWriteError
    public void onWriteError(Exception e, List<? extends BanNumber> items){
        log.error("An error occured in ##csvWriter##", e);
    }
}
// 2. implements를 사용해서 핸들링 가능

public class ErrorHandler implements ItemWriteListener{
    // ItemReaderListner 등을 상속받아서 @Override해서 처리 가능
    @Override
    void onWriteError(java.lang.Exception exception,java.util.List<? extends S> items){
        // 에러 처리
    }
}
```

## 다중 스레드 스텝
```java
@Bean
public Step csvFileItemRedisStep(){ // Redis에 파일 결과 저장
    return stepBuilderFactory.get("csvFileItemRedisStep")
            .<BanNumberDto,BanNumberRedis>chunk(CHUNK_SIZE)
            .reader(csvReader.csvFileItemReader())
            .processor(csvRedisConvert)
            .writer(csvRedisWriter)
            .listener(new CsvErrorListener())
            .taskExecutor(new SimpleAsyncTaskExecutor()) // 다중 스레딩 기능을 추가하는데 필요한 TaskExecutor 구현체 정의
            .build();
}
// .taskExecutor 다중 스레딩 기능을 추가하는데 필요한 TaskExecutor 구현체 
// new SimpleAsyncTaskExecutor() 스텝 내에서 실행되는 각 청크용으로 새 스레드를 생성해 각 청크를 병렬로 실행한다.
```
- 다중 스레드 환경에서 여러 스레드가 접근 가능한 상태를 가진 객체(동기화가 돼 있지 않는 등)에는 서로 다른 스레드의 상태로 덮어쓰게 되는 문제가 발생할 수 있다.
- taskexecutor로 성능을 개선시킬 수 있지만 입력 메커니즘이 네트워크, 디스크 버스 등과 같은 자원을 모두 소모하고 있다면 성능이 나아지지 않는다.

## 병렬 스텝
> 추가 예정
  
## 파티셔닝
> 추가 예정

## 원격청킹
> 추가 예정

#### 참고자료
##### 스프링배치 완벽 가이드(에이콘 출판)
