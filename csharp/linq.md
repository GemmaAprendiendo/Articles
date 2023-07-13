# Linq

## General

Linq uses deferred execution. You can use linq for arrays and links and others similar to those. I'll just use List for the examples. Some examples using linq. (I just have a List<string> of some random words).

```
var query1 = theList.Where(i => i.StartsWith("w"));
foreach (string w in query1.ToList())
{
  Console.WriteLine(w);
}
var query2 = theList.Where(i => i.Contains("ll"));
foreach (string w in query2.ToList())
{
  Console.WriteLine(w);
}
//using linq query syntax
var query3 = from word in theList where word.StartsWith("w") select word; 
foreach (string w in query3.ToList())
{
  Console.WriteLine(w);     
}
var query4 = from word in theList where word.Contains("ll") select word; 
foreach (string w in query4.ToList())
{
  Console.WriteLine(w);
}
```

## Filtering Sorting

Use System.Linq if it is not in the file already. You can also call a method that will indicate whether the word should go in or not.

```
/have this method: Takes string, returns bool
public bool ShouldWordGoIn(string word)
{
  return (word.Length > 5);
}

//note: we don't have to include the new Func..., we can just have  Where(methodName);
var query3 = theList.Where(new Func<string, bool>(ShouldWordGoIn));
foreach (string w in query4.ToList())
{
  Console.WriteLine(w);
}

//Since the code in the method is simple, could just have done This
var query4 = theList.Where(word => word.Length > 5);

//another way to do the previous one would be
var query5 = theList.Where(word => word.Length > 5).OrderByDescending(w => w.Length);

//doing this will take the ones longer than 5 and sort them alphabetically in descending order.
var query5 = theList.Where(word => word.Length > 5).OrderByDescending(w => w);

//do OrderBy(w => w); to get them by ascending alphabetical order.
```

You can also sort by the length and then by the name:

```
var query5 = theList.Where(word => word.Length > 5).OrderBy(w => w.Length).ThenBy(w => w);

//animal
//doctor
//enough
//fluoride
//fluoride1
//fluoride2
//animation1
//animation2
//animation3
//fluoride10
```

Linq also provides a way to filter based on type:` theList.OfType<WhateverType>`

## Sets

There are some operations that can be done that are similar to the ones we have for sets. If I have this:

```
theList1.Add("animal");
theList1.Add("animal");
theList1.Add("animation");
theList1.Add("animation");
theList1.Add("arm");                
theList1.Add("and");
theList1.Add("appropriate");
theList1.Add("astute");   
```

Getting the distinct ones will not show the duplicates: `var query1 = theList1.Distinct();`

For the same list, we can say we want the disctinct ones based on the first 2 chars: `ar query1 = theList1.DistinctBy( w => w.Substring(0, 2));`

Now I have the lists as:
```
theList1.Add("animal");
theList1.Add("arm");
theList1.Add("animation");
theList1.Add("and");
theList1.Add("appropriate");
theList1.Add("astute");
theList1.Add("animal");
theList1.Add("animation");

theList2.Add("baby");
theList2.Add("arm");
theList2.Add("beasts");
theList2.Add("appropriate");
```

` var query1 = theList1.Concat(theList2);` will get

```
animal
arm
animation
and
appropriate
astute
animal
animation
baby
arm
beasts
appropriate
```
Union is different from Concat. Union will get you the union without the duplicates.`var query1 = theList1.Union(theList2);`

You can also intersect `var query1 = theList1.Intersect(theList2);`

Also an except which also removed duplicates `var query1 = theList1.Intersect(theList2);`

ZIP will let you pairs of items from one list to the other (if they are not the same, the items without pair will be left out). Notice that the last 3 of the longer list (1) are not paired with anybody. (this is done by order)

`var query1 = theList1.Zip(theList2, (first, second) => first + " - " + second);`

```
animal - baby
arm - arm
animation - beasts
and - appropriate
```

## Linq with EF

```
using (Northwind db = new Northwind())
{
  DbSet<Product> products = db.Products;
  IQueryable<Product>? productsCh = products.Where(p => EF.Functions.Like(p.ProductName, "Ch%"));
  IOrderedQueryable<Product> sortedProds = productsCh.OrderByDescending(product => product.Cost);
  foreach (Product p in sortedProds)
  {
    Console.WriteLine(p.ProductName + "----" + p.cost.ToString());
  }
}
```

```
var someNewType = new
{
  ProductName = "I am new",
  Cost = 98.98
}
```

```
//notice we cannot use IQueryable<Product> anymore because we are using the anonymous type and not getting all cols back.
IQueryable? productsCh = products.Select(prod => new { prod.ProductId, prod.ProductName, prod.cost }).Where(p => EF.Functions.Like(p.ProductName, "Ch%"));

Then in the for loop use var:
foreach (var p in productsCh)
  ...
  Console.WriteLine(p.ToString());

```

```
DbSet<Product> products = db.Products;
DbSet<Category> categories = db.Categories;
 ...
//join categories with the products, based on the categoryID (outer key for the category, inner key for the product)
var join = categories.Join(inner: products, 
      outerKeySelector: category => category.CategoryId, 
      innerKeySelector: prod => prod.CategoryId, 
resultSelector: (c, p) => new {c.CategoryId, c.CategoryName, p.ProductName, p.cost});
```

## Parallel Linq

You can run queries with the .AsParallel to use different processors at the same time if you have them. Whatever comes after the .AsParallel will be run in parallel, so don't just put it at the end of the statement.

## Linq to XML
Pay attention to the types being used here or it won't work. Don't put the DBSet directly in the XML or you get an exception.

```
DbSet<Category> cats = db.Categories;
List<Category> catList = cats.ToList();

XElement xml = new XElement("CATEGORIES",  (from c in catList
  select new XElement("category", 
  new XAttribute("CatID", c.CategoryId),
  new XElement("desc", c.Description),
  new XElement("name", c.CategoryName)))
);
```

