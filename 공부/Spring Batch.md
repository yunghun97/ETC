# ๐ Spring Batch

 
### ๋์ฉ๋์ ๋ฐ์ดํฐ๋ฅผ ํจ์จ์ ์ผ๋ก ์ฒ๋ฆฌ
- ๋ค์ค ์ค๋ ๋, ๋ณ๋ ฌ ์คํ ์ฒ๋ฆฌ ๋ฑ ํจ์จ์ ์ธ ์ฒ๋ฆฌ ๊ฐ๋ฅ

### ๋ค์ํ ์ํฉ์ ๋ฐ๋ฅธ ๋์ฒ ๊ฐ๋ฅ
- skip, retry ๋ฑ ๋ค์ํ ์ํฉ์ ๋ฐ๋ฅธ ๋์ฒ๋ฅผ ํ  ์ ์์
- ๋ค์ํ listener๋ฅผ ๊ฐ์ง๊ณ  ์์ผ๋ฏ๋ก ์์ ์ , ํ ์๋ฌ ๋ฐ์์ ๋์ฒ ๊ฐ๋ฅ

### ํด๋น ๋ก์ง์ ๊ฒฐ๊ณผ๋ฅผ ์ง๊ด์  ์ ๊ณต
- ๊ธฐ๋ณธ์ ์ผ๋ก ์ ๊ณตํ๋ Spring Batch ํ์ด๋ธ์ ํตํด ์์์ ์คํ, ์๋ฃ ์๊ฐ, ์ฑ๊ณต, ์คํจ ํ์ธ ๊ฐ๋ฅ
  

## ๐ Job

- Job์ ๋ฐฐ์น์ฒ๋ฆฌ ๊ณผ์ ์ ํ๋์ ๋จ์๋ก ๋ง๋ค์ด ๋์ ๊ฐ์ฒด์๋๋ค. ์ฌ๋ฌ Step ์ธ์คํด์ค๋ฅผ ํฌํจํ๋ ์ปจํ์ด๋, ๋ฐฐ์น์ฒ๋ฆฌ ๊ณผ์ ์ ์์ด ์ ์ฒด ๊ณ์ธต ์ต์๋จ์ ์์นํ๊ณ  ์์ต๋๋ค. 

### JobInstance 
- Job์ ์คํ์ ๋จ์
- Job์ ์คํ์ํค๊ฒ ๋๋ฉด ํ๋์ JobInstance๊ฐ ์์ฑ๋ฉ๋๋ค.
- 1์ 1์ผ์ ์คํํ JobInstance๊ฐ ์คํจํ์ฌ ๋ค์ ์คํ ์ํค๋๋ผ๊ณ  1์ 1์ผ์ ๋ํ ๋ฐ์ดํฐ๋ง ์ฒ๋ฆฌํจ
  
### JobParameters
- JobInstance๋ฅผ JobParameters ๊ฐ์ฒด๋ก ๊ตฌ๋ถํ๊ฒ ๋ฉ๋๋ค.
- String, Double, Long, Date 4๊ฐ์ง ํ์๋ง์ ์ง์ํ๊ณ  ์์ต๋๋ค.
  
### JobExecution
- JobInstance ์คํ์ ์๋์ ๋ํ ๊ฐ์ฒด
- JobInstance ์คํ๋ง๋ค ๋ณ๋์ JobExecution์ด ์์ฑ๋๋ค.
- JobExecution์ JobInstance ์คํ์ ๋ํ ์ํ, ์์์๊ฐ, ์ข๋ฃ์๊ฐ, ์์ฑ์๊ฐ ๋ฑ์ ์ ๋ณผ๋ฅด ๋ด๊ณ  ์์ต๋๋ค.  
  
### JobRepository
- ๋ชจ๋  ๋ฐฐ์น ์ฒ๋ฆฌ ์ ๋ณด๋ฅผ ๋ด๊ณ ์๋ ๋งค์ปค๋์ฆ์๋๋ค.
- Job์ด ์คํ๋๊ฒ ๋๋ฉด JobRepository์ JobExecution๊ณผ StepExecution์ ์์ฑํ๊ฒ ๋๋ฉฐ JobRepository์์ Execution ์ ๋ณด๋ค์ ์ ์ฅํ๊ณ  ์กฐํํ๋ฉฐ ์ฌ์ฉํ๊ฒ ๋ฉ๋๋ค.
  
## ๐ญ Step

- Step์ Job์ ๋ฐฐ์น์ฒ๋ฆฌ๋ฅผ ์ ์ํ๊ณ  ์์ฐจ์ ์ธ ๋จ๊ณ๋ฅผ ์บก์ํ ํฉ๋๋ค. Job์ ์ต์ํ 1๊ฐ ์ด์์ Step์ ๊ฐ์ ธ์ผ ํ๋ฉฐ Job์ ์ค์  ์ผ๊ด ์ฒ๋ฆฌ๋ฅผ ์ ์ดํ๋ ๋ชจ๋  ์ ๋ณด๊ฐ ๋ค์ด์์ต๋๋ค.

### StepExecution
- JobExecution๊ณผ ๋์ผํ๊ฒ Step ์คํ ์๋์ ๋ํ ๊ฐ์ฒด๋ฅผ ๋ํ๋๋๋ค.
- ์ด์  ๋จ๊ณ์ Step  ์ด ์คํจํ๊ฒ ๋๋ฉด ๋ค์ ๋จ๊ณ๊ฐ ์คํ๋์ง ์์ผ๋ฏ๋ก ์๋ฌ ์ดํ์ StepExceution์ ์์ฑ๋์ง ์์ต๋๋ค.
- ์ค์  ์์์ด ๋ ๋๋ง ์์ฑ๋ฉ๋๋ค.
- JobExecution์ ์ ์ฅ๋๋ ์ ๋ณด ์ธ์ read ์, write ์, commit ์, skip ์ ๋ฑ์ ์ ๋ณด๋ค๋ ์ ์ฅ์ด ๋ฉ๋๋ค.
  
### ExecutionContext
- Job์์ ๋ฐ์ดํฐ๋ฅผ ๊ณต์ ํ  ์ ์๋ ๋ฐ์ดํฐ ์ ์ฅ์์๋๋ค.
- ExecutionContext๋ JobExecutionContext, StepExecutionContext 2๊ฐ์ง ์ข๋ฅ๊ฐ ์์ผ๋ ์ด ๋๊ฐ์ง๋ ์ง์ ๋๋ ๋ฒ์๊ฐ ๋ค๋ฆ๋๋ค. 
- JobExecutionContext์ ๊ฒฝ์ฐ Commit ์์ ์ ์ ์ฅ
- StepExecutionContext๋ ์คํ ์ฌ์ด์ ์ ์ฅ์ด ๋๊ฒ ๋ฉ๋๋ค.
- ExecutionContext๋ฅผ ํตํด Step๊ฐ Data ๊ณต์ ๊ฐ ๊ฐ๋ฅํ๋ฉฐ Job ์คํจ์ ExecutionContext๋ฅผ ํตํ ๋ง์ง๋ง ์คํ ๊ฐ์ ์ฌ๊ตฌ์ฑ ํ  ์ ์์ต๋๋ค.

![image](https://user-images.githubusercontent.com/71022555/179394027-029bd0ed-735f-47f3-b1f5-113e913812e9.png)  
  
## ๐ง Item

### ItemReader
- Step์์ Item์ ์ฝ์ด์ค๋ ์ธํฐํ์ด์ค์๋๋ค. ItemReader์ ๋ํ ๋ค์ํ ์ธํฐํ์ด์ค๊ฐ ์กด์ฌํ๋ฉฐ ๋ค์ํ ๋ฐฉ๋ฒ์ผ๋ก Item์ ์ฝ์ด ์ฌ ์ ์์ต๋๋ค.  

### ItemProcessor
- ItemReader์์ ์ฝ์ด์จ Item(๋ฐ์ดํฐ)๋ฅผ ์ฒ๋ฆฌํ๋ ์ญํ 
- Reader, Writer๋ ํ์์ง๋ง Processor๋ ํ์๊ฐ ์๋๋ค.
  
### ItemWriter
- ์ฒ๋ฆฌ๋ Data๋ฅผ Writer ํ  ๋ ์ฌ์ฉํ๋ค.

![image](https://user-images.githubusercontent.com/71022555/181202071-e4f8c371-69a7-4a0a-b38d-751f7ef73800.png)  
  

## ๐ ํ์ผ ์๋ ฅ

## ๐ ํ๋ซ ํ์ผ(Flat File)
- ํ ๊ฐ ๋๋ ๊ทธ ์ด์์ ๋ ์ฝ๋๊ฐ ํฌํจ๋ ํน์  ํ์ผ
- ํ์ผ ๋ด์ฉ์ ๋ด๋ ๋ฐ์ดํฐ์ ์๋ฏธ๋ฅผ ์ ์ ์๋ค๋ ์ ์์ XML ํ์ผ๊ณผ ์ฐจ์ด๊ฐ ์๋ค.
- ํ๋ซ ํ์ผ์๋ ํ์ผ ๋ด์ ๋ฐ์ดํฐ์ ํฌ๋งท์ด๋ ์๋ฏธ๋ฅผ ์ ์ํ๋ ๋ฉํ๋ฐ์ดํฐ๊ฐ ์๋ค.
- XML ํ์ผ์ ํ๊ทธ๋ฅผ ์ฌ์ฉํด ๋ฐ์ดํฐ์ ์๋ฏธ๋ฅผ ๋ถ์ฌํ๋ค.
- ํ๋ซ ํ์ผ์ ์ฒ๋ฆฌํ๋ ItemReader๋ฅผ ์์๋ณด๊ธฐ ์ ์ ์คํ๋ง ๋ฐฐ์น์์ ํ์ผ์ ์ฝ์ ๋ ์ฌ์ฉํ๋ ์ปดํฌ๋ํธ
- org.sprignframeowrk.batch.item.file.FlatFileItemReader๋ ๋ฉ์ธ ์ปดํฌ๋ํธ ๋ ๊ฐ๋ก ์ด๋ค์ง๋ค.
  - ์ฝ์ด๋ค์ผ ๋์ ํ์ผ์ ๋ํ๋ด๋ ์คํ๋ง์ Resource
  - org.springframewrok.batch.item.file.LineMapper ์ธํฐํ์ด์ค ๊ตฌํ์ฒด์ด๋ค.
   
### LineMapper
- JDBC์์ RowMapper๊ฐ ๋ด๋นํ๋ ๊ฒ๊ณผ ๋น์ทํ ์ญํ ์ ํ๋ค.
- ์คํ๋ง JDBC์์ RowMapper๋ฅผ ์ฌ์ฉํ๋ฉด ํ๋์ ๋ฌถ์์ ๋ํ๋ด๋ ResultSet์ ๊ฐ์ฒด๋ก ๋งคํํ  ์ ์๋ค.
- ํ์ผ์ ์ฝ์ ๋ ํ์ผ์์ ๋ ์ฝ๋ ํ ๊ฐ์ ํด๋นํ๋ ๋ฌธ์์ด์ด LineMapper ๊ตฌํ์ฒด์ ์ ๋ฌ๋๋ค.
- ๊ฐ์ฅ ๋ง์ด ์ฌ์ฉํ๋ LineMapper ๊ตฌํ์ฒด๋ DefaultLineMapper์ด๋ค.
  - DefaultLineMapper๋ ํ์ผ์์ ์ฝ์ ์์(raw) String์ ๋์์ผ๋ก ๋ ๋จ๊ณ ์ฒ๋ฆฌ(LineTokenizer, FieldSetMapper)๋ฅผ ๊ฑฐ์ณ ์ดํ ์ฒ๋ฆฌ์์ ์ฌ์ฉํ  ๋๋ฉ์ธ ๊ฐ์ฒด๋ก ๋ณํํ๋ค.

### LinkTokenizer 
- LinkTokenizer ๊ตฌํ์ฒด๊ฐ ํด๋น ์ค์ ํ์ฑํด org.springframework.batch.item.file.FieldSet์ผ๋ก ๋ง๋ ๋ค.
- LineTokenizer์ ์ ๊ณต๋๋ String์ ํ์ผ์์ ๊ฐ์ ธ์จ ํ ์ค ์ ์ฒด๋ฅผ ๋ํ๋ธ๋ค.
- ๋ ์ฝ๋ ๋ด์ ๊ฐ ํ๋๋ฅผ ๋๋ฉ์ธ ๊ฐ์ฒด๋ก ๋งคํํ๋ ค๋ฉด ํด๋น ์ค์ ํ์ฑํด ๊ฐ ํ๋๋ฅผ ๋ํ๋ด๋ ๋ฐ์ดํฐ์ ๋ชจ์์ผ๋ก ๋ณํํ  ์ ์์ด์ผ ํ๋ค.
- Spring Batch์ FieldSet์ ํ ์ค์ ํด๋นํ๋ ํ๋์ ๋ชจ์์ ๋ํ๋ธ๋ค. ์ด๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ก ์์ํ  ๋ ์ฌ์ฉํ๋ java.sql.ResultSet๊ณผ ์ ์ฌํ๋ค.

### FileSetMapper
- FileSetMapper ๊ตฌํ์ฒด๋ FileSet์ ๋๋ฉ์ธ ๊ฐ์ฒด๋ก ๋งคํํ๋ค.
- LineTokenizer๊ฐ ํ ์ค์ ์ฌ๋ฌ ํ๋๋ก ๋๋ด์ผ๋ฏ๋ก, ์ด์  ๊ฐ ์๋ ฅ ํ๋๋ฅผ ๋๋ฉ์ธ ๊ฐ์ฒด์ ํ๋๋ก ๋งคํํ  ์ ์๋ค.
- Spring JDBC์์ RowMapper๊ฐ ResultSet์ ๋ก์ฐ(row)๋ฅผ ๋๋ฉ์ธ ๊ฐ์ฒด๋ก ๋งคํํ๋ ๊ฒ๊ณผ ํก์ฌํ๋ค.

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
        /* delimitedLineTokenizer : setNames๋ฅผ ํตํด ๊ฐ๊ฐ์ ๋ฐ์ดํฐ์ ์ด๋ฆ ์ค์  */

        DelimitedLineTokenizer delimitedLineTokenizer = new DelimitedLineTokenizer(",");
        delimitedLineTokenizer.setNames("kind", "number", "use");
        defaultLineMapper.setLineTokenizer(delimitedLineTokenizer);
        
        /* beanWrapperFieldSetMapper : Tokenizer์์ ๊ฐ์ง๊ณ ์จ ๋ฐ์ดํฐ๋ค์ VO๋ก ๋ฐ์ธ๋ํ๋ ์ญํ  */
        BeanWrapperFieldSetMapper<BanNumberDto> beanWrapperFieldSetMapper = new BeanWrapperFieldSetMapper<>();
        beanWrapperFieldSetMapper.setTargetType(BanNumberDto.class);

        defaultLineMapper.setFieldSetMapper(beanWrapperFieldSetMapper);

        // LineMapper ์ง์ 
        flatFileItemReader.setLineMapper(defaultLineMapper);

        return flatFileItemReader;
    }
}
```


## ๐ ํ์ผ ์ถ๋ ฅ

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

## ๐ ์๋ฌ ์ฒ๋ฆฌ
```java
@Bean
public Step csvFileItemReaderStep() {
    return stepBuilderFactory.get("csvFileItemReaderStep")
            .<BanNumberDto, BanNumber>chunk(CHUNK_SIZE)
            .reader(csvReader.csvFileItemReader())
            .processor(csvConvert)
            .writer(csvWriter)
            .listener(new CsvErrorListener()) // ์๋ฌ ํธ๋ค๋ฌ
            .build();
}

// 1. Annotation ์ฌ์ฉํด์ ์๋ฌ ํธ๋ค๋ง
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
// 2. implements๋ฅผ ์ฌ์ฉํด์ ํธ๋ค๋ง ๊ฐ๋ฅ

public class ErrorHandler implements ItemWriteListener{
    // ItemReaderListner ๋ฑ์ ์์๋ฐ์์ @Overrideํด์ ์ฒ๋ฆฌ ๊ฐ๋ฅ
    @Override
    void onWriteError(java.lang.Exception exception,java.util.List<? extends S> items){
        // ์๋ฌ ์ฒ๋ฆฌ
    }
}
```

## ๋ค์ค ์ค๋ ๋ ์คํ
```java
@Bean
public Step csvFileItemRedisStep(){ // Redis์ ํ์ผ ๊ฒฐ๊ณผ ์ ์ฅ
    return stepBuilderFactory.get("csvFileItemRedisStep")
            .<BanNumberDto,BanNumberRedis>chunk(CHUNK_SIZE)
            .reader(csvReader.csvFileItemReader())
            .processor(csvRedisConvert)
            .writer(csvRedisWriter)
            .listener(new CsvErrorListener())
            .taskExecutor(new SimpleAsyncTaskExecutor()) // ๋ค์ค ์ค๋ ๋ฉ ๊ธฐ๋ฅ์ ์ถ๊ฐํ๋๋ฐ ํ์ํ TaskExecutor ๊ตฌํ์ฒด ์ ์
            .build();
}
// .taskExecutor ๋ค์ค ์ค๋ ๋ฉ ๊ธฐ๋ฅ์ ์ถ๊ฐํ๋๋ฐ ํ์ํ TaskExecutor ๊ตฌํ์ฒด 
// new SimpleAsyncTaskExecutor() ์คํ ๋ด์์ ์คํ๋๋ ๊ฐ ์ฒญํฌ์ฉ์ผ๋ก ์ ์ค๋ ๋๋ฅผ ์์ฑํด ๊ฐ ์ฒญํฌ๋ฅผ ๋ณ๋ ฌ๋ก ์คํํ๋ค.
```
- ๋ค์ค ์ค๋ ๋ ํ๊ฒฝ์์ ์ฌ๋ฌ ์ค๋ ๋๊ฐ ์ ๊ทผ ๊ฐ๋ฅํ ์ํ๋ฅผ ๊ฐ์ง ๊ฐ์ฒด(๋๊ธฐํ๊ฐ ๋ผ ์์ง ์๋ ๋ฑ)์๋ ์๋ก ๋ค๋ฅธ ์ค๋ ๋์ ์ํ๋ก ๋ฎ์ด์ฐ๊ฒ ๋๋ ๋ฌธ์ ๊ฐ ๋ฐ์ํ  ์ ์๋ค.
- taskexecutor๋ก ์ฑ๋ฅ์ ๊ฐ์ ์ํฌ ์ ์์ง๋ง ์๋ ฅ ๋ฉ์ปค๋์ฆ์ด ๋คํธ์ํฌ, ๋์คํฌ ๋ฒ์ค ๋ฑ๊ณผ ๊ฐ์ ์์์ ๋ชจ๋ ์๋ชจํ๊ณ  ์๋ค๋ฉด ์ฑ๋ฅ์ด ๋์์ง์ง ์๋๋ค.

## ๋ณ๋ ฌ ์คํ
> ์ถ๊ฐ ์์ 
  
## ํํฐ์๋
> ์ถ๊ฐ ์์ 

## ์๊ฒฉ์ฒญํน
> ์ถ๊ฐ ์์ 

#### ์ฐธ๊ณ ์๋ฃ
##### ์คํ๋ง๋ฐฐ์น ์๋ฒฝ ๊ฐ์ด๋(์์ด์ฝ ์ถํ)
