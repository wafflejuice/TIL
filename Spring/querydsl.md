# 2023-10-05
## Subquery supports in clauses
<table>
  <th></th>
  <th>where</th>
  <th>having</th>
  <th>select</th>
  <th>from</th>
  <tr>
    <td>JPQL</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Hibernate (before v6.1)</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
  <tr>
    <td>Hibernate (from v6.1)</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
  </tr>
  <tr>
    <td>Querydsl 5 + Hibernate</td>
    <td>O</td>
    <td>O</td>
    <td>O</td>
    <td>X</td>
  </tr>
</table>

Hibernate 6.1부터 from절의 subquery(inline view)가 가능해졌다. (https://in.relation.to/2022/06/14/orm-61-final/, https://hibernate.org/orm/releases/6.1/)

Querydsl 5에서는 아직 Hibernate subqueries in FROM clause를 지원하지 않는다. Querydsl측에서는 JPA specification을 기다리는 중이다. (https://github.com/querydsl/querydsl/issues/3438)
