# Java Persistence API

[JPA spec](https://download.oracle.com/otn-pub/jcp/persistence-2\_1-fr-eval-spec/JavaPersistence.pdf?AuthParam=1586960793\_29025f13537694baad23f9777fbe93c1)[spec](https://docs.oracle.com/javaee/5/tutorial/doc/bnbpz.html)\
JPA - specification which is used to access, manage and persist data between Java objects and relation database. Standard approach for object relational mapping.\
Main points

* @Entity - entity class
* @Id - every entity class have an id field
* @Table - define table name - not required
* @Column - column name not required
* @JoinColumn - is used to override the name of the foreign key column for relationship references
* @OneToMany @ManyToOne - bidirectional relationship pair, only one foreign key is required in one of the tables to manage both sides of the relationship.

\
**EntityManger** - is used to create new entities, prepare query to return sets of entities, modify entities.



```
EntityManagerFactory emf = Persistence .createEntityManagerFactory("PetShop"); 
EntityManager em = emf.createEntityManager(); 
... 
em.close();
```

```
 public class MyBean implements MyInterface { 
    @PersistenceContext(unitName="PetShop") 
    EntityManager em; 

}
```

```
public Pet createPet(int idNum, String name, PetType  type) { 
    Pet pet = new Pet(idNum, name, type); 
    em.persist(pet); 
    return pet; 
} 
public Pet findPet(int id) { 
    return em.find(Pet.class, id); 
} 
public Pet changeName(int id, String newName) {  
    Pet pet = this.findPet(id); 
    pet.setName(newName); 
    return pet; 
} 
public void deletePet(int id) { 
    Pet pet = this.findPet(id); 
    em.remove(pet); 
}
```

```
em.getTransaction().begin(); 
 Pet pet = new Pet(idNum, name, type); 
 em.persist(pet); 
 em.getTransaction().commit(); 
return pet;
```

querying

```
Query q = em.createQuery("SELECT p FROM Pet p");
List results = q.getResultList();

//named query
@NamedQuery(name="Pet.findByName",  query="SELECT p FROM Pet p WHERE p.name LIKE :pname") 
@Entity 
public class Pet { ... }

Query q = em.createNamedQuery("Pet.findByName"); 
q.setParameter("pname", petName); 
return q.getResultList();
```
