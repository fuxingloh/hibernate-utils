# Hibernate Utils
Hibernate utils for JPA entity manager, with this utils you can run transaction in a lambda.

### Supported Functions
- with() run transaction without returning anything.
- reduce() run transaction and return a object.
- optional() run transaction and return a optional object. NoResultException is caught and will return Optional.empty()

### Creating Transaction Provider
Provider are created with persistence.xml
```java

// Creating default provider with unitName = defaultPersistenceUnit
TransactionProvider provider = HibernateUtils.setupFactory(null);
// Get default provider from HibernateUtils
TransactionProvider provider = HibernateUtils.get();
// Shutting down default provider
HibernateUtils.shutdown();

// Custom provider with assigned unit name
TransactionProvider provider = HibernateUtils.setupFactory("myPersistenceUnit", null);
// Get custom provider from HibernateUtils
TransactionProvider provider = HibernateUtils.get("myPersistenceUnit");
// Shutting down custom provider
HibernateUtils.shutdown("myPersistenceUnit");

// Shutdown all provider
HibernateUtils.shutdownAll();

// Create provider with custom properties
Map<String, String> properties = new HashMap<>();
properties.put("hibernate.hbm2ddl.auto", "create-drop");
HibernateUtils.setupFactory("myPersistenceUnit", properties);
```

### Using transaction Provider
```java
// Setup persistence unit factory
HibernateUtils.setupFactory("myPersistenceUnit");

TransactionProvider provider = HibernateUtils.get("myPersistenceUnit");
// With function
provider.with(em -> em.persist(row));
// Reduce function
provider.reduce(em -> em.createQuery(
                "SELECT COUNT(DISTINCT g.id) FROM Row g", Long.class)
                .getSingleResult());
// Optional function
provider.optional(em -> em.createQuery(
                "SELECT g FROM Row g WHERE g.id = 1234", Long.class)
                .getSingleResult());
```