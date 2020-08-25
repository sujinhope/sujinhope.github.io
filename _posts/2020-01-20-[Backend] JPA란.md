# JPA란? (SpringBoot + JPA 사용법)

### <b>JPA란?</b>

**Java Persistance API.** 자바의 **ORM**(Object-Relational Mapping) 표준 기술이다. 즉 자바의 객체와 관계형 DB를 매핑하는 기술이다.

<br/>

### <b>Hibernate</b>

 Hibernate란? ORM Framework 중 하나이다. JDBC를 이용하다가 MyBatis를 이용하면 훨씬 편하고 코드가 간결하며 유지보수가 편하다. 마찬가지로 Hibernate도 MyBatis에 비해 코드가 훨씬 더 간결하며 객체지향적이다.

 구글 지역별 비교 분석에서 볼 수 있듯이 한국에서는 MyBatis를 많이 사용하지만 전세계 개발자들은 Hibernate를 많이 사용한다.

<center><img src="https://user-images.githubusercontent.com/33229855/72794524-7afaee80-3c7f-11ea-903e-474675a2d7c9.png"/></center>

<br/><br/>

**장점**

1. 생산성
2. 유지보수

<br/>

**단점**

1. 어렵다
2. 성능상 문제가 있을 수 있다.


<br/>

### <b>사용법</b>

 MyBatis나 기존에 자바에서 RDBMS 내의 테이블 데이터를 출력하기 위해서는 <b>SELECT * FROM USER;</b> 라는 query를 실행해야 했다. 하지만 JPA를 이용할 경우 <b>user.findAll()</b> 이라는 간단한 자바 명령어로 역할을 대신할 수 있다.

 DTO 에서 객체와 DB 테이블을 매핑해주고, Repository를 만들고 나면 Controller에서 JPA에서 제공하는 몇 가지 기본적인 메소드들을 사용할 수 있다. 기본적으로는 아래 세가지를 포함한 몇가지 함수들을 제공한다.

- save()
- findAll()
- deleteAll()

<br/><br/>

 만일 커스텀 기능을 사용하고 싶을 경우 Repository에 직접 선언 후 사용할 수 있다. 예를 들어 FAQRepository에 faq 번호(no)로 검색하는 쿼리문을 동작시키고 싶을 경우 아래와 같이 함수 이름을 **findByNo(int no)**로 선언해주기만 하면 아래 쿼리문과 똑같이 동작하는 기능을 사용할 수 있다.

```sql
select * from faq where no=#{no}
```



![image](https://user-images.githubusercontent.com/33229855/72769517-4b77c200-3c3e-11ea-824d-d8a063f57f7f.png)

<br/>

​	FAQController에서는 아래와 같이 @Autowired를 통해 FAQRepository를 연결시켜주고 repo.findByNo(no)를 통해 함수를 수행할 수 동작시킬 수 있다.

![image](https://user-images.githubusercontent.com/33229855/72769613-a6111e00-3c3e-11ea-9c5e-bc44c3758be0.png)



<br/>

#### **폴더 구조**

- 크게 DTO, Controller, Repository, Service로 구성된다.
- Repository는 인터페이스로

![image-20200121105027929](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200121105027929.png)

<br/>

##### <b>application.properties</b>  

```properties
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp


spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://70.12.247.81:3306/ssafysnsdb?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true&characterEncoding=utf-8
spring.datasource.username=ssafy
spring.datasource.password=ssafy
spring.datasource.type=org.apache.commons.dbcp.BasicDataSource

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.database=mysql
# 아래와 같이 mysql InnoDB 설정을 해줘야 foreign key가 작동한다. (default는 MyISAM)
spring.jpa.database-platform: org.hibernate.dialect.MySQL5InnoDBDialect
```

<br/><br/>

#### <b>DTO Annotation 사용법</b>  

1. ##### DTO 위

   생성자 접근 권한은 PUBLIC, PROTECTED 외 다른 것들을 지정할 수 있다. 단, PROTECTED를 설정할 경우 외부에서 해당 생성자에게 접근하는 것을 막을 수 있다. 

   ```java
   @AllArgsConstructor									      //1. 모든 인자가 있는 생성자
   @NoArgsConstructor(access=AccessLevel.PROTECTED)	//2. 인자 없는 생성자
   @ToString						//3. toString() 메소드 생성
   @Getter							//4. @Getter, @Setter 대신 @Data로 사용할 수도 있다.
   @Setter
   @Entity							//5. DB테이블과 일대일로 매칭되는 객체 단위
   @Table(name="notice")		//6. Notice 객체와 매핑될 테이블 이름 지정
   public class Notice {		//5,6을 합쳐서 @Entity(name="notice")로 사용 가능.
   								   //따로 지정해주지 않을 경우 class 명과 동일하게 지어진다.
   }    
   ```

   

2. ##### DTO 내부 - 속성 선언

   ```java
   	@Id						                              //테이블 primary key 지정
   	@GeneratedValue(strategy=GenerationType.IDENTITY)	//auto_increment
   	@Column(length=30, nullable=false, unique=true)		//타입 크기, not null, unique
   	private int no;
   	
   	@Column(length=50, nullable=false)
   	private String title;
   	
   	@Column(length=1000, nullable=false)
   	private String content;
   	
   	@ManyToOne				   //Foreign Key 지정 - User(id)를 참조할 경우
   	@JoinColumn(name="id", foreignKey=@ForeignKey(name="fk_notice_user_id"))
   	private User user;		//User타입을 가져오는 것에 주의!!
   	
      //Date 타입 format 지정
   	@JsonFormat(shape=JsonFormat.Shape.STRING, pattern="yyyy-MM-dd HH:mm:ss", timezone="Asia/Seoul")
   	@Column(nullable=false)
   	private Date datetime;
   	
   	@Column(nullable=false)
   	private boolean deleted;
   ```

<br/><br/>



3. ##### <b> 그 외 Annotation</b>

   3-1. 객체 위에 사용하는 Annotation

   ```java
   //모든 파라미터가 있는 생성자
   @AllArgsConstructor
   
   //not null인 애들 빼고 모두 파라미터로 가지는 생성자
   @RequiredArgsConstructor
   
   ```

   <br/>

   3-2. 속성 위에 사용하는 Annotation

   ```java
   //exclude - 특정 컬럼을 뺄 수 있음.
   @ToString(exclude="컬럼명")
   
   //Not null 지정. 이걸 이용할 경우 자바상에서도 에러를 잡아준다.
   @NotNull
   
   //DB타입에 맞춰서 매핑해줌.
   @Temporal(TemporalType.DATE)
   private Date birth;
   
   //Boolean Type을 사용하고 싶을 경우
   @Column(columnDefinition = "TINYINT", length=1)
   private int deleted;
   
   ```

   <br/>


   전체 DTO 예제

   ```java
   package com.ssafysns.model.dto;
   
   import java.util.Date;				//java.util.Date를 import 해야 한다.
   
   import javax.persistence.Column;
   import javax.persistence.Entity;
   import javax.persistence.ForeignKey;
   import javax.persistence.GeneratedValue;
   import javax.persistence.GenerationType;
   import javax.persistence.Id;
   import javax.persistence.JoinColumn;
   import javax.persistence.ManyToOne;
   import javax.persistence.Table;
   
   
   import com.fasterxml.jackson.annotation.JsonFormat;
   
   import lombok.AccessLevel;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import lombok.ToString;
   
   //생성자 접근 권한 - PUBLIC, PACKAGE
   @NoArgsConstructor(access=AccessLevel.PROTECTED)
   @ToString
   @Data
   @Entity
   @Table(name="notice")
   public class Notice {
   	
   	@Id
   	@GeneratedValue(strategy=GenerationType.IDENTITY)
   	@Column(length=30, nullable=false, unique=true)
   	private int no;
   	
   	@Column(length=50, nullable=false)
   	private String title;
   	
   	@Column(length=1000, nullable=false)
   	private String content;
   	
   	@ManyToOne
   	@JoinColumn(name="id", foreignKey=@ForeignKey(name="fk_notice_user_id"))
   	private User user;
   	
   	@JsonFormat(shape=JsonFormat.Shape.STRING, pattern="yyyy-MM-dd HH:mm:ss", timezone="Asia/Seoul")
   	@Column(nullable=false)
   	private Date datetime;
   	
   	@Column(nullable=false)
   	private boolean deleted;
   
   }
   ```

















