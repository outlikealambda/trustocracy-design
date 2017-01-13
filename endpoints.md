# App

### Per Topic
(topicId supplied via uri)

get trustee opinions
```
[
  { 
    trustee,
    Maybe(
      numHops,
      author,
      opinion
    )
  }
]

[
  { 
    person {
      name,
      id,
      relation
      ?rank
    },
    ?author(person){
      name,
      id,
      relation
      ?rank
    },
    ?opinion  
  }
]
```    

set trusted
```
{
  topicId,
  authorId
}
```

vouch for author
```
{
  authorId
}

