---
title: SprngSecurity实战实例，JWT Token登录认证&权限控制
date: 2022-11-03 06:31:33
categories: Spring生态
tags: [原创, java, 项目实战, SpringSecurity,JWT,权限控制]
cover: https://img2.baidu.com/it/u=924906505,2741718824&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=250
---

> 这个在暑假就写过，上学以后重新再去写一遍发现困难比较多，于是准备写一篇记录一下。
>
> tips: 源码在文章末尾，sql文件在源码resource文件夹下的sql文件夹里 

# 需求

想用springsecurity实现JWT Token登录认证，实现动态菜单生成，权限控制，登出（用户注销）。

# 认证

## 准备工作

1. maven依赖

   ```xml
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.47</version>
           </dependency>
    		<dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.0</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-security</artifactId>
           </dependency>
   
           <dependency>
               <groupId>io.jsonwebtoken</groupId>
               <artifactId>jjwt</artifactId>
               <version>0.9.1</version>
           </dependency>
   
           <!--redis依赖-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-data-redis</artifactId>
           </dependency>
   ```

2. 添加Redis相关配置

   ```java
   import com.alibaba.fastjson.JSON;
   import com.alibaba.fastjson.serializer.SerializerFeature;
   import com.fasterxml.jackson.databind.JavaType;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.fasterxml.jackson.databind.type.TypeFactory;
   import org.springframework.data.redis.serializer.RedisSerializer;
   import org.springframework.data.redis.serializer.SerializationException;
   import com.alibaba.fastjson.parser.ParserConfig;
   import org.springframework.util.Assert;
   import java.nio.charset.Charset;
   
   /**
    * Redis使用FastJson序列化
    * 
    * @author sg
    */
   public class FastJsonRedisSerializer<T> implements RedisSerializer<T>
   {
   
       public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");
   
       private Class<T> clazz;
   
       static
       {
           ParserConfig.getGlobalInstance().setAutoTypeSupport(true);
       }
   
       public FastJsonRedisSerializer(Class<T> clazz)
       {
           super();
           this.clazz = clazz;
       }
   
       @Override
       public byte[] serialize(T t) throws SerializationException
       {
           if (t == null)
           {
               return new byte[0];
           }
           return JSON.toJSONString(t, SerializerFeature.WriteClassName).getBytes(DEFAULT_CHARSET);
       }
   
       @Override
       public T deserialize(byte[] bytes) throws SerializationException
       {
           if (bytes == null || bytes.length <= 0)
           {
               return null;
           }
           String str = new String(bytes, DEFAULT_CHARSET);
   
           return JSON.parseObject(str, clazz);
       }
   
   
       protected JavaType getJavaType(Class<?> clazz)
       {
           return TypeFactory.defaultInstance().constructType(clazz);
       }
   }
   ```

3.  响应类

   ```java
   public class RespBean {
       private Integer status;
       private String msg;
       private Object object;
   
       public static RespBean ok(String msg) {
           return new RespBean(200, msg, null);
       }
   
       public static RespBean ok(String msg, Object obj) {
           return new RespBean(200, msg, obj);
       }
   
   
       public static RespBean error(String msg) {
           return new RespBean(500, msg, null);
       }
   
       public static RespBean error(String msg, Object obj) {
           return new RespBean(500, msg, obj);
       }
   
       public static RespBean error() {
           return new RespBean();
       }
   
   
       public RespBean(Integer status, String msg) {
           this.status = status;
           this.msg = msg;
       }
   
       public RespBean() {
           this.status = 500;
           this.msg = "操作异常";
       }
   
   
       public RespBean(Integer status, String msg, Object object) {
           this.status = status;
           this.msg = msg;
           this.object = object;
       }
   
       public Integer getStatus() {
           return status;
       }
   
       public void setStatus(Integer status) {
           this.status = status;
       }
   
       public String getMsg() {
           return msg;
       }
   
   
       public void setMsg(String msg) {
           this.msg = msg;
       }
   
       public Object getObject() {
           return object;
       }
   
   
   }
   ```

4. 工具类

   ```java
   
   import io.jsonwebtoken.Claims;
   import io.jsonwebtoken.JwtBuilder;
   import io.jsonwebtoken.Jwts;
   import io.jsonwebtoken.SignatureAlgorithm;
   
   import javax.crypto.SecretKey;
   import javax.crypto.spec.SecretKeySpec;
   import java.util.Base64;
   import java.util.Date;
   import java.util.UUID;
   
   /**
    * JWT工具类
    */
   public class JwtUtil {
   
       //有效期为
       public static final Long JWT_TTL = 60 * 60 *1000L;// 60 * 60 *1000  一个小时
       //设置秘钥明文
       public static final String JWT_KEY = "lcdzzz";
   
       public static String getUUID(){
           String token = UUID.randomUUID().toString().replaceAll("-", "");
           return token;
       }
       
       /**
        * 生成jtw
        * @param subject token中要存放的数据（json格式）
        * @return
        */
       public static String createJWT(String subject) {
           JwtBuilder builder = getJwtBuilder(subject, null, getUUID());// 设置过期时间
           return builder.compact();
       }
   
       /**
        * 生成jtw
        * @param subject token中要存放的数据（json格式）
        * @param ttlMillis token超时时间
        * @return
        */
       public static String createJWT(String subject, Long ttlMillis) {
           JwtBuilder builder = getJwtBuilder(subject, ttlMillis, getUUID());// 设置过期时间
           return builder.compact();
       }
   
       private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid) {
           SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
           SecretKey secretKey = generalKey();
           long nowMillis = System.currentTimeMillis();
           Date now = new Date(nowMillis);
           if(ttlMillis==null){
               ttlMillis=JwtUtil.JWT_TTL;
           }
           long expMillis = nowMillis + ttlMillis;
           Date expDate = new Date(expMillis);
           return Jwts.builder()
                   .setId(uuid)              //唯一的ID
                   .setSubject(subject)   // 主题  可以是JSON数据
                   .setIssuer("lcdzzz")     // 签发者
                   .setIssuedAt(now)      // 签发时间
                   .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                   .setExpiration(expDate);
       }
   
       /**
        * 创建token
        * @param id
        * @param subject
        * @param ttlMillis
        * @return
        */
       public static String createJWT(String id, String subject, Long ttlMillis) {
           JwtBuilder builder = getJwtBuilder(subject, ttlMillis, id);// 设置过期时间
           return builder.compact();
       }
   
       public static void main(String[] args) throws Exception {
   //        String jwt = createJWT("2123");
           Claims claims = parseJWT("eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyOTY2ZGE3NGYyZGM0ZDAxOGU1OWYwNjBkYmZkMjZhMSIsInN1YiI6IjIiLCJpc3MiOiJzZyIsImlhdCI6MTYzOTk2MjU1MCwiZXhwIjoxNjM5OTY2MTUwfQ.NluqZnyJ0gHz-2wBIari2r3XpPp06UMn4JS2sWHILs0");
           String subject = claims.getSubject();
           System.out.println(subject);
   //        System.out.println(claims);
       }
   
       /**
        * 生成加密后的秘钥 secretKey
        * @return
        */
       public static SecretKey generalKey() {
           byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
           SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
           return key;
       }
       
       /**
        * 解析
        *
        * @param jwt
        * @return
        * @throws Exception
        */
       public static Claims parseJWT(String jwt) throws Exception {
           SecretKey secretKey = generalKey();
           return Jwts.parser()
                   .setSigningKey(secretKey)
                   .parseClaimsJws(jwt)
                   .getBody();
       }
   
   
   }
   ```

   ```java
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.data.redis.core.BoundSetOperations;
   import org.springframework.data.redis.core.HashOperations;
   import org.springframework.data.redis.core.RedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;
   import org.springframework.stereotype.Component;
   
   import java.util.*;
   import java.util.concurrent.TimeUnit;
   
   @SuppressWarnings(value = { "unchecked", "rawtypes" })
   @Component
   public class RedisCache
   {
       @Autowired
       private RedisTemplate redisTemplate;
   
   
       /**
        * 缓存基本的对象，Integer、String、实体类等
        *
        * @param key 缓存的键值
        * @param value 缓存的值
        */
       public <T> void setCacheObject(final String key, final T value)
       {
           redisTemplate.opsForValue().set(key, value);
       }
   
       /**
        * 缓存基本的对象，Integer、String、实体类等
        *
        * @param key 缓存的键值
        * @param value 缓存的值
        * @param timeout 时间
        * @param timeUnit 时间颗粒度
        */
       public <T> void setCacheObject(final String key, final T value, final Integer timeout, final TimeUnit timeUnit)
       {
           redisTemplate.opsForValue().set(key, value, timeout, timeUnit);
       }
   
       /**
        * 设置有效时间
        *
        * @param key Redis键
        * @param timeout 超时时间
        * @return true=设置成功；false=设置失败
        */
       public boolean expire(final String key, final long timeout)
       {
           return expire(key, timeout, TimeUnit.SECONDS);
       }
   
       /**
        * 设置有效时间
        *
        * @param key Redis键
        * @param timeout 超时时间
        * @param unit 时间单位
        * @return true=设置成功；false=设置失败
        */
       public boolean expire(final String key, final long timeout, final TimeUnit unit)
       {
           return redisTemplate.expire(key, timeout, unit);
       }
   
       /**
        * 获得缓存的基本对象。
        *
        * @param key 缓存键值
        * @return 缓存键值对应的数据
        */
       public <T> T getCacheObject(final String key)
       {
           ValueOperations<String, T> operation = redisTemplate.opsForValue();
           return operation.get(key);
       }
   
       /**
        * 删除单个对象
        *
        * @param key
        */
       public boolean deleteObject(final String key)
       {
           return redisTemplate.delete(key);
       }
   
       /**
        * 删除集合对象
        *
        * @param collection 多个对象
        * @return
        */
       public long deleteObject(final Collection collection)
       {
           return redisTemplate.delete(collection);
       }
   
       /**
        * 缓存List数据
        *
        * @param key 缓存的键值
        * @param dataList 待缓存的List数据
        * @return 缓存的对象
        */
       public <T> long setCacheList(final String key, final List<T> dataList)
       {
           Long count = redisTemplate.opsForList().rightPushAll(key, dataList);
           return count == null ? 0 : count;
       }
   
       /**
        * 获得缓存的list对象
        *
        * @param key 缓存的键值
        * @return 缓存键值对应的数据
        */
       public <T> List<T> getCacheList(final String key)
       {
           return redisTemplate.opsForList().range(key, 0, -1);
       }
   
       /**
        * 缓存Set
        *
        * @param key 缓存键值
        * @param dataSet 缓存的数据
        * @return 缓存数据的对象
        */
       public <T> BoundSetOperations<String, T> setCacheSet(final String key, final Set<T> dataSet)
       {
           BoundSetOperations<String, T> setOperation = redisTemplate.boundSetOps(key);
           Iterator<T> it = dataSet.iterator();
           while (it.hasNext())
           {
               setOperation.add(it.next());
           }
           return setOperation;
       }
   
       /**
        * 获得缓存的set
        *
        * @param key
        * @return
        */
       public <T> Set<T> getCacheSet(final String key)
       {
           return redisTemplate.opsForSet().members(key);
       }
   
       /**
        * 缓存Map
        *
        * @param key
        * @param dataMap
        */
       public <T> void setCacheMap(final String key, final Map<String, T> dataMap)
       {
           if (dataMap != null) {
               redisTemplate.opsForHash().putAll(key, dataMap);
           }
       }
   
       /**
        * 获得缓存的Map
        *
        * @param key
        * @return
        */
       public <T> Map<String, T> getCacheMap(final String key)
       {
           return redisTemplate.opsForHash().entries(key);
       }
   
       /**
        * 往Hash中存入数据
        *
        * @param key Redis键
        * @param hKey Hash键
        * @param value 值
        */
       public <T> void setCacheMapValue(final String key, final String hKey, final T value)
       {
           redisTemplate.opsForHash().put(key, hKey, value);
       }
   
       /**
        * 获取Hash中的数据
        *
        * @param key Redis键
        * @param hKey Hash键
        * @return Hash中的对象
        */
       public <T> T getCacheMapValue(final String key, final String hKey)
       {
           HashOperations<String, String, T> opsForHash = redisTemplate.opsForHash();
           return opsForHash.get(key, hKey);
       }
   
       /**
        * 删除Hash中的数据
        *
        * @param key
        * @param hkey
        */
       public void delCacheMapValue(final String key, final String hkey)
       {
           HashOperations hashOperations = redisTemplate.opsForHash();
           hashOperations.delete(key, hkey);
       }
   
       /**
        * 获取多个Hash中的数据
        *
        * @param key Redis键
        * @param hKeys Hash键集合
        * @return Hash对象集合
        */
       public <T> List<T> getMultiCacheMapValue(final String key, final Collection<Object> hKeys)
       {
           return redisTemplate.opsForHash().multiGet(key, hKeys);
       }
   
       /**
        * 获得缓存的基本对象列表
        *
        * @param pattern 字符串前缀
        * @return 对象列表
        */
       public Collection<String> keys(final String pattern)
       {
           return redisTemplate.keys(pattern);
       }
   }
   ```

5. 实体类

   ```java
   @Data
   @NoArgsConstructor
   @AllArgsConstructor
   
   public class User implements UserDetails, Serializable {
   
       private static final long serialVersionUID = 1L;
   
       @TableId(value = "id", type = IdType.AUTO)
       private Long id;
   
       private String username;
   
       private String password;
   
       private String name;
   
       private Integer sex;
   
       /**
        * 医保卡号
        */
       private String medicalInsuranceCardNumber;
   
       /**
        * 身份证号
        */
       private String idNumber;
   
       private String phone;
       private String salt;
   
       private LocalDateTime gmtCreate;
   
       private LocalDateTime gmtModified;
       @TableLogic
       private Integer isDeleted;
   
   }
   ```



## 实现

### 配置工作

```sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `sex` int(11) DEFAULT NULL,
  `medical_insurance_card_number` varchar(255) DEFAULT NULL COMMENT '医保卡号',
  `id_number` varchar(255) DEFAULT NULL COMMENT '身份证号',
  `phone` varchar(255) DEFAULT NULL,
  `salt` varchar(255) DEFAULT NULL,
  `gmt_create` datetime DEFAULT NULL,
  `gmt_modified` datetime DEFAULT NULL,
  `is_deleted` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8;

```

数据库配置信息

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/smart_reg??useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: wqeq
```

<br>

### 定义mapper接口

```java
@Repository
public interface UserMapper extends BaseMapper<User> {
```

<br>

### 实体类

关于实体类，mapper层自动生成可以看https://lcdzzz.github.io/2022/06/22/mybatisplus/ 的”代码生成器部分“

实体类需要实现UserDetails接口，并实现里面的方法

```java

@Data
@NoArgsConstructor
@AllArgsConstructor

public class User implements UserDetails, Serializable {

    private static final long serialVersionUID = 1L;

    @TableId(value = "id", type = IdType.AUTO)
    private Long id;

    private String username;

    private String password;

    private String name;

    private Integer sex;

    /**
     * 医保卡号
     */
    private String medicalInsuranceCardNumber;

    /**
     * 身份证号
     */
    private String idNumber;

    private String phone;
    private String salt;

    private LocalDateTime gmtCreate;

    private LocalDateTime gmtModified;
    @TableLogic
    private Integer isDeleted;

    /**
    	先这么写，在授权一块会补全
    */
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {

        return true;
    }
    @TableField(exist = false)//：表示该属性不为数据库表字段，但又是必须使用的。
    private List<Role> roles;

}

```

mapper扫描

```java
@MapperScan("com.qingshan.smartregsystem.mapper")
@SpringBootApplication
public class SmartRegSystemApplication {

    public static void main(String[] args) {
        SpringApplication.run(SmartRegSystemApplication.class, args);
    }

}
```

添加junit依赖

```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
```

 测试MP是否能正常使用

```java
@SpringBootTest
public class MapperTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testUserMapper(){
        List<User> users = userMapper.selectList(null);
        System.out.println(users);
    }
}
```

<br>

### controller层

```java
 
@RestController
@RequestMapping("/smartregsystem/user")
public class LoginController {
    @Autowired
    LoginServcie loginServcie;

    @PostMapping("/login")
    public RespBean login(@RequestBody User user) {
        //登录
        return loginServcie.login(user);
    }


```

### service类（核心代码)

```java
@Service
public class LoginServiceImpl implements LoginServcie {

    @Autowired
    private AuthenticationManager authenticationManager;

    @Autowired
    UserMapper userMapper;

    @Autowired
    private RedisCache redisCache;

    @Override
    public RespBean login(User user) {
        //AuthenticationManager authenticate进行用户认证
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUsername(),user.getPassword());
        Authentication authenticate = authenticationManager.authenticate(authenticationToken);
        //如果认证没通过，给出对应的提示
        if(Objects.isNull(authenticate)){
            throw new RuntimeException("登录失败");
        }
        //如果认证通过了，使用userid生成一个jwt jwt存入ResponseResult返回
        User loginUser = (User) authenticate.getPrincipal();

        String userid = loginUser.getId().toString();
        loginUser.setPassword(null);
        String jwt = JwtUtil.createJWT(userid);
        Map<String,Object> map = new HashMap<>();
        map.put("token",jwt);
        map.put("userInfo",loginUser);
        //把完整的用户信息存入redis  userid作为key
        redisCache.setCacheObject("login:"+userid,loginUser);
        return  RespBean.ok("登录成功",map);
    }
}
```

执行到`Authentication authenticate = authenticationManager.authenticate(authenticationToken);`这行时，需要通过`UserDetailsService`这个接口的实现类去完成用户认证。所以需要一个类去实现`UserDetailsService`，并实现`loadUserByUsername`方法。

ps:这里内含最开始没有用springsecurity实现注册的痕迹，可以随便看看，用了MD5加密的方法，不过用了springsecurity以后，就用它自带的`BCryptPasswordEncoder`了。关于怎么配置，在后面的`SecurityConfig`配置中会有说明。

`user.setRoles(userMapper.getHrRolesById(user.getId()));`这行代码可以先不要，这是接下来授权部分的代码。

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements IUserService, UserDetailsService {
    @Autowired
    UserMapper userMapper;

     @Override
    public UserDetails loadUserByUsername(String username)  {
        //查询用户信息
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.eq("username", username);
        User user = userMapper.selectOne(queryWrapper);
//        User user = userMapper.selectByUserName(username);
        //如果没有查询到用户就抛出异常
        if(Objects.isNull(user)){
            System.out.println("没有查到用户！！！");
        }
        user.setRoles(userMapper.getHrRolesById(user.getId()));
        //把数据封装成UserDetails返回
        return user;
    }
    
    @Override
    public Integer reg(User user) {
        String username = user.getUsername();
        User result=userMapper.selectAllByUsername(username);
        if (result!=null){
            throw new UserRegOrLoginException("用户名被占用");
        }
// 创建当前时间对象
        LocalDateTime now = LocalDateTime.now();
        /**
         * 密码加密处理的实现：md5算法
         */
        // 补全数据：加密后的密码
        String salt = UUID.randomUUID().toString().toUpperCase();
        BCryptPasswordEncoder encoder = new BCryptPasswordEncoder();
        String encodePassword = encoder.encode(user.getPassword());
//        String md5Password = getMd5Password(user.getPassword(), salt);
        user.setPassword(encodePassword);
        // 补全数据：盐值
        user.setSalt(salt);
//        // 补全数据：isDelete(0)
//        user.setIsDelete(0);
        // 补全数据：4项日志属性

        user.setGmtCreate(now);
        user.setGmtModified(now);
        user.setIsDeleted(0);
        Integer rows = userMapper.insert(user);
        if (rows!=1){
            throw new UserRegOrLoginException("在用户注册过程中产生了未知异常");
        }
        return rows;
    }

   
}
```

验证完，LoginServiceImpl的login方法就可以继续往下走了

<br>

### 关于密码加密存储

实际项目中我们不会把密码明文存储在数据库中。

我们一般使用SpringSecurity为我们提供的BCryptPasswordEncoder。

只需要使用把BCryptPasswordEncoder对象注入Spring容器中，SpringSecurity就会使用该PasswordEncoder来进行密码校验。

可以定义一个SpringSecurity的配置类，SpringSecurity要求这个配置类要继承WebSecurityConfigurerAdapter。

同时这也是非常重要的springsecurity配置类

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {


    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }

}
```

### springsecurity配置类

在这之前我们自定义登陆接口，所以需要让SpringSecurity对这个接口放行,让用户访问这个接口的时候不用登录也能访问。

在接口中我们通过AuthenticationManager的authenticate方法来进行用户认证,所以也需要在SecurityConfig中配置把AuthenticationManager注入容器。（上面是把BCryptPasswordEncoder注入进容器）

认证成功的话要生成一个jwt，放入响应中返回。并且为了让用户下回请求时能通过jwt识别出具体的是哪个用户，我们需要把用户信息存入redis，可以把用户id作为key。(代码可见上面LoginServiceImpl类的login方法)

ps:这边复制完会在` http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);`上报错，别急，继续往下看

```java

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    JwtAuthenticationTokenFilter jwtAuthenticationTokenFilter;

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();//特点：相同的明文，加密后生成的密文是不一样的
        //return NoOpPasswordEncoder.getInstance();   //该密码编码器为字符串匹配，不做加密比较
    }


    //配置路径的拦截规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/smartregsystem/user/login").permitAll()
                .antMatchers("/smartregsystem/user/reg").permitAll()
                .anyRequest().authenticated()
               
        ;


        // 除上面外的所有请求全部需要鉴权认证

        //把token校验过滤器添加到过滤器链中
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);

        //允许跨域
        http.cors();
    }
   
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}

```

### 认证过滤器

在上面springsecurity配置类中，我们把token校验过滤器也添加到了过滤器链中` http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);` 

说明：

- 我们需要自定义一个过滤器，这个过滤器会去获取请求头中的token，对token进行解析取出其中的userid。
- 使用userid去redis中获取对应的LoginUser对象。
- 然后封装Authentication对象存入SecurityContextHolder

```java
@Component
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter {

    @Autowired
    private RedisCache redisCache;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        //获取token
        String token = request.getHeader("token");
        if (!StringUtils.hasText(token)) {
            //放行
            filterChain.doFilter(request, response);
            return;
        }
        //解析token
        String userid;
        try {
            Claims claims = JwtUtil.parseJWT(token);
            userid = claims.getSubject();
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("token非法");
        }
        //从redis中获取用户信息
        String redisKey = "login:" + userid;
        User loginUser = redisCache.getCacheObject(redisKey);
        if(Objects.isNull(loginUser)){
            throw new RuntimeException("用户未登录");
        }

        //TODO 获取权限信息封装到AuthenticationToken中
        //存入SecurityContextHolder
        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(loginUser,null,null);
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        //放行
        
        filterChain.doFilter(request, response);
    }
}
```

<br>

### 退出登录

我们只需要定义一个退出登录的接口，然后获取SecurityContextHolder中的认证信息，删除redis中对应的数据即可。

```java
@GetMapping("/logout")
public RespBean logout(){
    return loginServcie.logout();
}
```

```java
@Override
public RespBean logout() {
    //获取SecurityContextHolder中的用户id
    UsernamePasswordAuthenticationToken authentication = (UsernamePasswordAuthenticationToken) SecurityContextHolder.getContext().getAuthentication();
    User loginUser = (User) authentication.getPrincipal();
    Long userid = loginUser.getId();
    //删除redis中的值
    redisCache.deleteObject("login:"+userid);
    return  RespBean.ok("注销成功");
}
```

<br>

> 到这一步，登录和登出代码已经完成，用户登录时输入username（账号），password（密码），就会返回用户信息和token。这里因为功能已经，所以有roles和authorities等信息，可以暂时忽略（接下来会讲）。

运行结果：

<div style="text-align:center">
  <img src="SprngSecurity实战实例，JWT Token登录认证&权限控制/image-20221103080526828.png" alt="image-20221103080526828" style="width:70%">
</div>

<div style="text-align:center">
  <img src="SprngSecurity实战实例，JWT Token登录认证&权限控制/image-20221103081846910.png" alt="image-20221103081846910" style="width:70%">
</div>



# 授权

##  授权基本流程

​	在SpringSecurity中，会使用默认的FilterSecurityInterceptor来进行权限校验。在FilterSecurityInterceptor中会从SecurityContextHolder获取其中的Authentication，然后获取其中的权限信息。当前用户是否拥有访问当前资源所需的权限。

​	所以我们在项目中只需要把当前登录用户的权限信息也存入Authentication。

​	然后设置我们的资源所需要的权限即可。

## 授权实现

### 封装权限信息

之前在在写UserDetailsServiceImpl的时候说过`user.setRoles(userMapper.getHrRolesById(user.getId()));`这行代码。

它的意思是在查询出用户后还要获取对应的权限信息，封装到UserDetails中返回。

讲到这里有必要说下这边数据库的设计，详细的可以去了解下RBAC（Role Based Access Control，基于角色的访问控制）权限模型，这里就不阐述了。

> 整个代码的仓库地址会放在文章末尾，里面会附带sql这个项目的整个数据库，数据都是假的，别管

<div style="text-align:center">
  <img src="SprngSecurity实战实例，JWT Token登录认证&权限控制/image-20221103084557831.png" alt="image-20221103084557831" style="width:70%">
</div>

<br>

sql语句:

```sql
 <select id="getHrRolesById" resultType="com.qingshan.smartregsystem.pojo.Role">
          SELECT r.* FROM role r,user_role ur WHERE ur.`rid`=r.`id` AND ur.`uid`=#{id}
 </select>
```

<br>

我们之前定义了UserDetails的实现类User，想要让其能封装权限信息就要对其进行修改。

实体类：

```java

@Data
@NoArgsConstructor
@AllArgsConstructor

public class User implements UserDetails, Serializable {

    private static final long serialVersionUID = 1L;

    @TableId(value = "id", type = IdType.AUTO)
    private Long id;

    private String username;

    private String password;

    private String name;

    private Integer sex;

    /**
     * 医保卡号
     */
    private String medicalInsuranceCardNumber;

    /**
     * 身份证号
     */
    private String idNumber;

    private String phone;
    private String salt;

    private LocalDateTime gmtCreate;

    private LocalDateTime gmtModified;
    @TableLogic
    private Integer isDeleted;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        List<SimpleGrantedAuthority> authorities =new ArrayList<>(roles.size());
        for (Role role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getName()));
        }
        return authorities;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {

        return true;
    }
    @TableField(exist = false)//：表示该属性不为数据库表字段，但又是必须使用的。
    private List<Role> roles;

}

```



User修改完后我们就可以在UserDetailsServiceImpl中去把权限信息封装到User中了。

<div style="text-align:center">
  <img src="SprngSecurity实战实例，JWT Token登录认证&权限控制/image-20221103092055878.png" alt="image-20221103092055878" style="width:70%">
</div>

### 所必要的配置类

`CustomFilterInvocationSecurityMetadataSource`和`CustomUrlDecisionManager`，这也是实现权限控制（根据权限对接口进行拦截）的核心。

<br>

#### CustomFilterInvocationSecurityMetadataSource

它作用是 根据用户传来的请求地址 分析出请求需要的角色

这里需要把登录的接口、获取动态菜单的接口、注册用户的接口，设置后“特殊”的权限，这样在后面就可以对这些需要“特殊”权限的接口放行。

```java

// 根据用户传来的请求地址 分析出请求需要的角色
@Component //注册为组件
public class CustomFilterInvocationSecurityMetadataSource implements FilterInvocationSecurityMetadataSource {
    @Autowired
    IMenuService menuService;
    //比对工具，这里用来比对request的url和menu的url

    AntPathMatcher antPathMatcher = new AntPathMatcher();

    @Autowired
    PathMatcher pathMatcher;

    @Override
    public Collection<ConfigAttribute> getAttributes(Object object) throws IllegalArgumentException {
        //当前请求的地址
        String requestUrl = ((FilterInvocation) object).getRequestUrl();
        if (requestUrl.equals("/smartregsystem/user/login")) {
            return SecurityConfig.createList("ROLE_WANT_TO_LOGIN");
        } else if (requestUrl.equals("/smartregsystem/config/menu")) {
//            System.out.println("给/smartregsystem/config/menu路径WANT_TO_MENU权限");
            return SecurityConfig.createList("ROLE_WANT_TO_MENU");
        }else if (requestUrl.equals("/smartregsystem/user/reg")) {
//            System.out.println("给/smartregsystem/config/menu路径WANT_TO_MENU权限");
            return SecurityConfig.createList("ROLE_WANT_TO_REG");
        }

        List<Menu> menus = menuService.getAllMenusWithRole();
        for (Menu menu : menus) {
            //match()中第一个是匹配规则，第二个是需要匹配的对象
            if ( pathMatcher.match(menu.getUrl(),requestUrl)){
                List<Role> roles = menu.getRoles();
                String[] str = new String[roles.size()];
                for (int i = 0; i < roles.size(); i++) {
                    str[i] = roles.get(i).getName();
                }
                return SecurityConfig.createList(str);
            }
        }
        //没有匹配上的 登陆后访问           标记 后续判断用
        return SecurityConfig.createList("ROLE_LOGIN");
    }

    @Override
    public Collection<ConfigAttribute> getAllConfigAttributes() {
        return null;
    }

    @Override
    public boolean supports(Class<?> aClass) {
        return true;
    }
}

```

<br>

#### CustomUrlDecisionManager

它的作用是判断当前用户是否具备CustomFilterInvocationSecurityMetadataSource分析出来的角色

在CustomFilterInvocationSecurityMetadataSource的时候，我们已经把登录、菜单、注册这三个接口附上了“需要特殊权限”的这个信息，而在这个类中，我们可以设置：假设用户需要的权限是 那三个特殊的权限，也就是：`ROLE_WANT_TO_LOGIN`、`ROLE_WANT_TO_MENU`、`ROLE_WANT_TO_REG`，就放行。

如果不是那三个特殊的权限，就会继续往下运行。

于是：

<div style="text-align:center">
  <img src="SprngSecurity实战实例，JWT Token登录认证&权限控制/image-20221103095154377.png" alt="image-20221103095154377" style="width:70%">
</div>

贴上代码↓

```java

/**
 * 判断当前用户是否具备CustomFilterInvocationSecurityMetadataSource分析出来的角色
 */
@Component   //注册为组件
public class CustomUrlDecisionManager implements AccessDecisionManager {
    /**
     *
     * @param authentication 用户的登陆登录信息
     * @param object
     * @param configAttributes CustomFilterInvocationSecurityMetadataSource分析出来的角色
     * @throws AccessDeniedException
     * @throws InsufficientAuthenticationException
     */
    @Override
    public void decide(Authentication authentication, Object object, Collection<ConfigAttribute> configAttributes)
            throws AccessDeniedException, InsufficientAuthenticationException {
        for (ConfigAttribute configAttribute : configAttributes) {
            //用户所需角色
            String needRole = configAttribute.getAttribute();
            if ("ROLE_LOGIN".equals(needRole)){
                //判断当前用户是否是匿名用户
                if (authentication instanceof AnonymousAuthenticationToken){
                    throw new AccessDeniedException("尚未登录，请登录！");
                }else {
                    return;
                }
            }
            if ("ROLE_WANT_TO_LOGIN".equals(needRole)){
                //当前请求时登录请求，放开
                return;
            }
            if ("ROLE_WANT_TO_MENU".equals(needRole)){
                //当前请求时菜单，放开
                return;
            }
            if ("ROLE_WANT_TO_REG".equals(needRole)){
                //当前请求时菜单，放开
                return;
            }
            //获取当前登录用户的角色

//            Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();
            List<Role> authorities= ((User) SecurityContextHolder.getContext().getAuthentication().getPrincipal()).getRoles();
            System.out.println("这里是authentication = " + authentication);
            for (Role authority : authorities) {
                //假如请求需要的角色是A、B，而我登录的角色是B、C，那也一样可以访问
                if (authority.getName().equals(needRole)){//遍历【注意：是双重循环，上面还有一重】，只要判断是否包含configAttributes集合中的任意一项
                    return;
                }
            }
        }
            throw new AccessDeniedException("权限不足，请联系管理员");

    }

    @Override
    public boolean supports(ConfigAttribute configAttribute) {
        return true;
    }

    @Override
    public boolean supports(Class<?> aClass) {
        return true;
    }
}


```





### SecurityConfig收尾工作

然后，我们需要在configure(HttpSecurity http)方法中，通过withObjectPostProcessor将刚刚创建CustomUrlDecisionManager和CustomFilterInvocationSecurityMetadataSource注入进来

```java

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    CustomUrlDecisionManager customUrlDecisionManager;
    @Autowired
    CustomFilterInvocationSecurityMetadataSource customFilterInvocationSecurityMetadataSource;

    @Autowired
    JwtAuthenticationTokenFilter jwtAuthenticationTokenFilter;

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();//特点：相同的明文，加密后生成的密文是不一样的
        //return NoOpPasswordEncoder.getInstance();   //该密码编码器为字符串匹配，不做加密比较
    }


    //配置路径的拦截规则
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/smartregsystem/user/login").permitAll()
                .antMatchers("/smartregsystem/user/reg").permitAll()
                .anyRequest().authenticated()
                .withObjectPostProcessor(new ObjectPostProcessor<FilterSecurityInterceptor>() {
                    @Override
                    public <O extends FilterSecurityInterceptor> O postProcess(O object) {
                        object.setAccessDecisionManager(customUrlDecisionManager);
                        object.setSecurityMetadataSource(customFilterInvocationSecurityMetadataSource);
                        return object;
                    }
                })
        ;


        // 除上面外的所有请求全部需要鉴权认证

        //把token校验过滤器添加到过滤器链中
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);



        //允许跨域
        http.cors();
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}

```

<br>

# 自定义失败处理

我们还希望在认证失败或者是授权失败的情况下也能和我们的接口一样返回相同结构的json，这样可以让前端能对响应进行统一的处理。要实现这个功能我们需要知道SpringSecurity的异常处理机制。

​	在SpringSecurity中，如果我们在认证或者授权的过程中出现了异常会被ExceptionTranslationFilter捕获到。在ExceptionTranslationFilter中会去判断是认证失败还是授权失败出现的异常。

> 对ExceptionTranslationFilter原理感兴趣的可以看这篇博客的第三部分https://blog.csdn.net/weixin_52834606/article/details/126729690

​	如果是认证过程中出现的异常会被封装成AuthenticationException然后调用**AuthenticationEntryPoint**对象的方法去进行异常处理。

​	如果是授权过程中出现的异常会被封装成AccessDeniedException然后调用**AccessDeniedHandler**对象的方法去进行异常处理。

​	所以如果我们需要自定义异常处理，我们只需要自定义AuthenticationEntryPoint和AccessDeniedHandler然后配置给SpringSecurity即可。



1. 自定义实现类

   ```java
   
   @Component
   public class AccessDeniedHandlerImpl implements AccessDeniedHandler {
       @Override
       public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException, ServletException {
           RespBean result = new RespBean(HttpStatus.FORBIDDEN.value(),"您的权限不足");
           String json = JSON.toJSONString(result);
           //处理异常
           WebUtils.renderString(response,json);
       }
   }
   
   ```

   ```java
   
   @Component
   public class AuthenticationEntryPointImpl implements AuthenticationEntryPoint {
       @Override
       public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
           RespBean result = new RespBean(HttpStatus.UNAUTHORIZED.value(),"用户认证失败请查询登录");
           String json = JSON.toJSONString(result);
           //处理异常
           WebUtils.renderString(response,json);
       }
   }
   ```

2. 配置给SpringSecurity

   先注入对应的处理器

   ```java
    @Autowired
       private AuthenticationEntryPoint authenticationEntryPoint;
   
       @Autowired
       private AccessDeniedHandler accessDeniedHandler;
   ```

   

   然后可以使用HttpSecurity对象的方法去配置。

   ps: 注释掉的是之前没有把两个实现类分离出来的写法，可以参考参考，当然更优雅的写法还是注释外的那种。

   ```java
    .and().exceptionHandling()
                   /*.authenticationEntryPoint((request, response, authenticationException) -> {
                       Map<String, Object> rs = new HashMap<>();
                       rs.put("code", 401);//注销以后再用直接的token去访问，就是这个异常
                       rs.put("msg", "尚未认证!");
                       ObjectMapper objectMapper = new ObjectMapper();
                       String json = objectMapper.writeValueAsString(rs);
                       response.setStatus(200);
                       response.setContentType(MediaType.APPLICATION_JSON_UTF8_VALUE);
                       response.getWriter().println(json);
   
                   })
                   .accessDeniedHandler((request, response, accessDeniedException) -> {
                       Map<String, Object> rs = new HashMap<>();
                       rs.put("code", 403);
                       rs.put("msg", "没有权限!");
                       ObjectMapper objectMapper = new ObjectMapper();
                       String json = objectMapper.writeValueAsString(rs);
                       response.setStatus(200);
                       response.setContentType(MediaType.APPLICATION_JSON_UTF8_VALUE);
                       response.getWriter().println(json);
                   })*/
                   .authenticationEntryPoint(authenticationEntryPoint).
                   accessDeniedHandler(accessDeniedHandler)
   ```

<br>

# 结尾

[源码地址](https://gitee.com/lcdzzz/smart-reg-system)

写本博客时，参考了

[spring Security 异常处理](https://blog.csdn.net/weixin_52834606/article/details/126729690 )

[SpringSecurity框架教程](https://www.bilibili.com/video/BV1mm4y1X7Hc/?p=37&spm_id_from=333.851.header_right.history_list.click&vd_source=11ab70dabc8b4eb287b19dc051f326bf)



最后还要感谢 http://www.javaboy.org/ 松哥的各种文章，没有他的指导，我没法在暑假写出动态权限的功能

还有关于springsecurity的其他细节，可以看↓

[一个案例演示 Spring Security 中粒度超细的权限控制](https://mp.weixin.qq.com/s/98CIxDVrhAiE14EXvgf7Vw) 

[Spring Security 中最流行的权限管理模型](https://mp.weixin.qq.com/s/B03cDEE1i3gT0yDhIxGATw )

[Spring Security 权限管理的投票器与表决机制](https://mp.weixin.qq.com/s/sU97RQjQq2-XXQt49LkSeQ) 









