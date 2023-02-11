##We make use of caching concept to get quick response

1st Run 'reids-server' file then only we can able to work on Redis

In caching what happen is when we call an API for 1st time It will fetch date from DB after that it will store
that fetched result in cache memory, when we make the same call then it will not go to the DB, rather it 
will search in cache memory 1st if the result for that particular request is present then it will return that 
result only, by avoiding, going to DB Hence it will increase the response time

#Note :To use Redis
1st download Redis-x64-3.2.100(exe file)

#MainClass
==========
@EnableCaching in main class

In EntityPackage don't use @Entity

#Application properties 
=======================
server.port=8080

#Dependency :
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

#ServiceImpl
============

@Autowired
private RedisTemplate template;

public Product findProductById(int id){
System.out.println("called findProductById from DB");
return (Product) template.opsForHash().get(Product,id);//Product:EntityName, id:passingValue(#Requesting value)
}


#Controller
===========
@GetMapping("/{id}")
@Cacheable(key = "#id",value = "Product",unless = "#result.price>1000")
//Here key :passing value, value="EntityName", unless is used to customize the evaluation, here .price is
fieldName of Product Entity
public Product findProduct(@PathVariable int id) {
return serviceImpl.findProductById(id);
}

#If we want to perform 'deleteById', and we want to reflect that result in cache
(or "to store the result in cache")
then use @CacheEvict
just above the 'deleteById' method

#If we want to update something DB, and we want to reflect that result in cache then us @CachePut
just above the save/update method

#If we want to perform getById, and we want to reflect that result in cache then us @Cacheable
just above the getById method


