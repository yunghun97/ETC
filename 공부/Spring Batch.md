# 😀 Spring Batch

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

#### 참고자료
##### 스프링배치 완벽 가이드(에이콘 출판)
