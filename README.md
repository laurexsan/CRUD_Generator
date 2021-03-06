<a name="top"/>		

# CRUD_Generator	
Code to read the Link to SQL auto generated types and create a CRUD class for each one of them.

# Table of Contents	
* [How to use](#howto)	
* [Observations](#observations)
* [Exemples](#exemples)	
	* [Files](#files)	
   	* [Data Base](#database)		
   	* [Create](#create)	
   	* [Read](#read)		
	* [Update](#update)		
   	* [Delete](#delete)		
* [Contributions](#contributions) 	

<a name="howto"/>

# How to use 
1. Copy the folder CODE to your project.

2. Create a namespace to be used ONLY by the designer.cs created by the Linq to SQL clases, or else will create a CRUD file to every type inside the namespace given.

3. Inside your main call the function Generate from the CRUDGenerator.cs passing a folder where to save the files and the namespace for de designer.cs from the Linq to SQL classes.

4. Run.

5. Open the folder where the clases where save and insert them into your project.

6. Done! You can delete the func call in your main.

<a name="observations"/>

# Observations 

This code is always assuming:

* that the first **type** in your **design.cs** file (created by Linq to SQL) is de **Data Context** to access the data base.

<a name="exemples"/>

# Exemples 

<a name="files"/>

### This are my files:
![alt text](https://github.com/laurexsan/CRUD_Generator/blob/master/Misc/01.jpg)

[To top](#top)

<a name="database"/>

### This is my test data base:
![alt text](https://github.com/laurexsan/CRUD_Generator/blob/master/Misc/02.jpg)

[To top](#top)

<a name="create"/>

### CREATE (inserting into your data base)

#### Generated code:
```csharp
static public Misc.Model.artista_ Create(int projeto_atual_id_, int? usuario_id_, string titulo_)
{
    	//See if finds a equal instance in the data base, if true, return it, or else insert a new one
	Misc.Model.artista_ artista_OBJ = ReadWhereFirst(new QueryConditions(projeto_atual_id_, OperatorComparer.Equals, "projeto_atual_id_"), new QueryConditions(usuario_id_, OperatorComparer.Equals, "usuario_id_"), new QueryConditions(titulo_, OperatorComparer.Equals, "titulo_"));

	if (artista_OBJ == null)
	{
		try
		{
			SistemaVotacaoDBDataContext context = new SistemaVotacaoDBDataContext();

			artista_OBJ = new Misc.Model.artista_();

			artista_OBJ.projeto_atual_id_ = projeto_atual_id_;
			artista_OBJ.usuario_id_ = usuario_id_;
			artista_OBJ.titulo_ = titulo_;

			context.artista_.InsertOnSubmit(artista_OBJ);
			context.SubmitChanges();
			return artista_OBJ;
		}
		catch (Exception error)
		{
			Console.WriteLine("EXCEPTION!!! " + "Classe: " + "mdl_artista_" + " Metódo: " + "Create");
		}
	}
	return artista_OBJ;
}

static public Misc.Model.artista_ Create(Misc.Model.artista_ artista_OBJ)
{
	return Create(artista_OBJ.projeto_atual_id_, artista_OBJ.usuario_id_, artista_OBJ.titulo_);
}
```

#### How to call: 
```csharp
Model.CRUD.model_artista_.Create(1, null, "Test");
```
*OR*
```csharp    
//Send the whole object (it will ignores the PKs)
Model.CRUD.model_artista_.Create(artista_OBJ);
```
[To top](#top)

<a name="read"/>

### READ (select data)

#### Generated code:
```csharp
static public IEnumerable<Misc.Model.artista_> ReadAllWhere(params QueryConditions[] conditions)
{
	try
	{
		SistemaVotacaoDBDataContext context = new SistemaVotacaoDBDataContext();

		IQueryable <Misc.Model.artista_> query = context.artista_;

		//This generate lambda expressions form the QueryConditions array
		foreach (QueryConditions q in conditions)
			query = query.Where(ExpressionBuilder.BuildPredicate<artista_>(q.value, q.comparer, q.properties));

		if (query.Any())
			return query;
		}
	catch (Exception erro)
	{
		Console.WriteLine("EXCEPTION!!! " + "Classe: " + "mdl_artista_" + " Metódo: " + "ReadAllWhere");
	}
	return null;
}
```

#### How to call:
```csharp
//QueryConditions recieves the value to compare, te type of comparison and the name of the atribute to compare as params
Model.CRUD.model_artista_.ReadAllWhere(new LauraStuffs.QueryConditions(1, LauraStuffs.OperatorComparer.Equals, "id_"));
```
[To top](#top)


<a name="update"/>

### UPDATE (update atribute values)
    
#### Generated code:
```csharp
static public Misc.Model.artista_ Update(int id_, int projeto_atual_id_, int? usuario_id_, string titulo_)
{
	try
	{
		SistemaVotacaoDBDataContext context = new SistemaVotacaoDBDataContext();

		var query = from u in context.artista_ where u.id_ == id_ select u;

		//If the instance esxists in the database, update it and return the new value, else return null
		if (query.Any())
		{
			Misc.Model.artista_ artista_OBJ = query.First();

			artista_OBJ.projeto_atual_id_ = projeto_atual_id_;
			artista_OBJ.usuario_id_ = usuario_id_;
			artista_OBJ.titulo_ = titulo_;

			context.SubmitChanges();
			return artista_OBJ;
		}
	}
	catch (Exception error)
	{
		Console.WriteLine("EXCEPTION!!! " + "Classe: " + "mdl_artista_" + " Metódo: " + "Update");
	}
	return null;
}

static public Misc.Model.artista_ Update(Misc.Model.artista_ artista_OBJ)
{
	return Update(artista_OBJ.id_, artista_OBJ.projeto_atual_id_, artista_OBJ.usuario_id_, artista_OBJ.titulo_);
}
```
 
#### How to call:
```csharp
//Send the id or the identity of the instance to be updated and the new atributes values
Model.CRUD.model_artista_.Update(1, 2, 1, "Test2");
```
*OR*
```csharp    
//Send the whole object
Model.CRUD.model_artista_.Update(artista_OBJ);
```

[To top](#top)

<a name="delete"/>

### DELETE (delete instance in the data base)

#### Generated code:
```csharp
static public bool Delete(int id_)
{
	try
	{
		SistemaVotacaoDBDataContext context = new SistemaVotacaoDBDataContext();

		var query = from u in context.artista_ where u.id_ == id_ select u;

		if (query.Any())
	{
			context.artista_.DeleteOnSubmit(query.First());
	return true;
	}
	}
	catch (Exception error)
	{
		Console.WriteLine("EXCEPTION!!! " + "Classe: " + "mdl_artista_" + " Metódo: " + "Delete");
	}
	return false;
}

static public bool Delete(Misc.Model.artista_ artista_OBJ)
{
	return Delete(artista_OBJ.id_);
}
```
		
#### How to call:
```csharp    
//Send the id or the identity of the instance to be deleted
Model.CRUD.model_artista_.Update(1, 2, 1, "Test2");
```
*OR*
```csharp    
//Send the whole object
Model.CRUD.model_artista_.Delete(artista_OBJ);
```

[To top](#top)

<a name="contributions"/>

# Contributions

The ExpressionPredicateBuilder.cs file Was created by this guy [user3411327](https://stackoverflow.com/users/3411327/user3411327) 
and it was found in this Stack Overflow [thread](https://stackoverflow.com/questions/22672050/dynamic-expression-tree-to-filter-on-nested-collection-properties/22685407#22685407) !!!

**Go say thanks to him ^^/!!!**

**THE END :D!** 
**ENJOY!**

[*Laura San Martin*](https://github.com/laurexsan/)	

lauragabrielasan@gmail.com
