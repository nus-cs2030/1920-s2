<frontmatter>
  pageNav: 2
  header: header.md
  footer: footer.md
  siteNav: site-nav.md
</frontmatter>

<br> 

# Unbounded Wildcards
<br> 

<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->
[Edit the material here! :fas-pen:](https://github.com/nus-cs2030/1920-s2/edit/master/contents/textbook/lecture05/unboundWildcards/unboundWildcards.md)
<!-- DO NOT DELETE THIS LINK AND PLEASE WRITE BELOW THIS LINK-->

This was adapted from [Question 462](https://piazza.com/class/k54zo22zq1t2zc?cid=462) from Piazza. 

`<?>` is known as the unbounded wildcard. This is used to infer to unknown types.  So, `List<?>` inherently means **a List of unknown types** This be the mantra of the day. 

Let us break it down. `List<?>` means a List of unknown types. If it is a List of unknown types, you should be able to insert a string into this list. However, this means you are inserting an object with a **known type**, which breaks the mantra. `List<?>` has almost the same meaning as `List<? extends Object>`, which means you are only able to **take out objects** of which you do not know the data type. 

As a result, you only can add `null` values into `List<?>`, since `null` is actually an unknown data type. As a result, we should not use `List<?>` to insert data into it. We should only use it when we want to refer to something of unknown data type inside the `List<?>`

You can read more about `<?>` in the [Java documentation](https://docs.oracle.com/javase/tutorial/java/generics/unboundedWildcards.html)! 

