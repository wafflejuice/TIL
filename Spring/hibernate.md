# 2023-09-02
## Hibernate 6: Fetch Join automatically applies `distinct`
As of Hibernate 6, duplicate results arising from the use of join fetch are automatically removed by Hibernate in memory, after reading the database results and materializing entity instances as Java objects. It’s no longer necessary to remove duplicate results explicitly, and, in particular, distinct should not be used for this purpose.

source: https://docs.jboss.org/hibernate/orm/current/userguide/html_single/Hibernate_User_Guide.html#hql-distinct
