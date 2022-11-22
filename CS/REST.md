# 2022-11-22
## HATEOAS
HATEOAS : Hypermedia As The Engine Of Application State

> 하이퍼미디어의 특징을 이용하여 HTTP 응답에 다음 액션이나 관계된 리소스에 대한 HTTP 링크를 함께 반환하는 것이다.

```json
{
    "id":"terry",
    "links":[
        {
            "rel":"friends",
            "href":"http://xxx/users/terry/friends"
        }
    ]
}
```

"대용량 아키텍처와 성능 튜닝", 조대협, p.154-155
