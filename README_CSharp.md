

# Backelite - Guide de Style CSharp

## Sources 

* [C# Coding Standards and Naming Conventions](http://www.dofactory.com/reference/csharp-coding-standards)

## Table des matières

* [Language](#language)
* [Nommage](#nommage)
* * [Classes et Méthodes](#classes-et-méthodes)
* * [Variables locales](#variables-locales)
* * [Structures de contrôle](#structures-de-contrôle)
* * [Opérateur ternaire](#opérateur-ternaire)
* [Organisation du code](#organisation-du-code)
* [Projet Windows](#projet-windows)


## Language

US English should be used.

**Par exemple:**
```
var myColor = new SolidColorBrush(Colors.White);
```

**Non pas:**
```
var myCouleur = new SolidColorBrush(Colors.White);
```

## Nommage

Il ne faut pas utiliser la [notation hongroise](https://fr.wikipedia.org/wiki/Notation_hongroise).

**Par exemple:**

```
int counter;
string name;
```

**Non pas:**

```
int iCounter;
string strName;
```

Utiliser les types prédéfinis au lieu des types de système comme Int16, Single, UInt64...

**Par exemple:**

```
string firstName;
int lastIndex;
bool isSaved;
```

**Non pas:**

```
String firstName;
Int32 lastIndex;
Boolean isSaved;
```

Utiliser le type implicite 'var' pour les déclarations de variables locales.

**Par exemple:**

```
var stream = File.Create(path);
var customers = new Dictionary();
 
// Exceptions
int index = 100;
string timeSheet;
bool isCompleted;
```

### Classes et Méthodes

Utiliser PascalCasing pour les noms de classes et les noms de méthode.

**Par exemple:**

```
public class ClientActivity
{
    public void ClearStatistics()
    {
        //...
    }
    public void CalculateStatistics()
    {
        //...
    }
}
```

Pour les interfaces, même règles que les classes sauf qu'on doit en plus commencer par un I majuscule.

**Par exemple:**

```
public interface IStringService
{
   
}
```

### Variables locales

Utiliser camelCasing pour les arguments d'une méthode et les variables locales.

**Par exemple:**

```
public class UserLog
{
    public void Add(LogEvent logEvent)
    {
        int itemCount = logEvent.Items.Count;
        // ...
    }
}
```

### Structures de contrôle

On retourne toujours à la ligne après une structure de contrôle pour placer les accolades.
On retourne également à la ligne après une accolade.

**Par exemple:**

```
if (condition)
{
    /*Code*/
}
else
{
    /*Code*/
}
 
for (...)
{
    /*Code*/
}
```


#### Conditions

*	**TOUJOURS** utiliser des accolades

**Par exemple:**

```
if (!error) 
{
    return success;
}
```

**Non pas:**

```
if (!error) return success;

if (!error) 
	return success;
```

* Si le mot clef est en rapport avec l'accolade fermante précédente on reviens quand même ligne (else, while du do…while)
* On met en espace entre le mot clef et la parenthèse qui le suit
* Et un saut de ligne après l'accolade fermante de la condition

**Par exemple:**

```
if (!error) 
{
    return success;	
} 
else 
{
    return failed;
}

Debug.WriteLine("");
```

**Non pas: (méga combo d'erreurs)**

```
if(!error){
    return success;
} else return failed;
Debug.WriteLine("");
```

* Mutliples conditions, placer l'opérateur en début de ligne.

**Par exemple:**

```
if (some_long_condition 
    && some_other_long_condition 
	|| some_completely_different_long_condition) 
{
	
}
```

* Mutliples conditions et conditions vraiment bien compliqué
**Par exemple:**

```
BOOL test1 = some_long_condition && some_other_long_condition;
BOOL test2 = some_other_long_condition && some_other_long_condition2;

if (test1 || test2) 
{
    ...
}
```

**Non pas:**

```
if (some_long_condition && some_other_long_condition || (some_other_long_condition && some_other_long_condition2) ||
    some_completely_different_long_condition) 
{
    
}
```

### Opérateur ternaire

L'opérateur ternaire, `?` , doit seulement être utilisé s'il rend le code plus lisible ou propre. Il doit seulement évaluer une condition simple. Évaluer plusieurs conditions est généralement plus facile à comprendre avec une condition de type if, ou refactorisé avec des variables nommées.

**Par exemple:**
```
result = a > b ? x : y;
```

**Non pas:**
```
result = a > b ? x = c > d ? c : d : y;
```

## Organisation du code

### Classes

Les variables statiques sont déclarées en haut de la classes, avant les variables globables.

**Par exemple:**

```
// Correct
public class Account
{
    public static string BankName;
    public static decimal Reserves;
 
    public string Number {get; set;}
    public DateTime DateOpened {get; set;}
    public DateTime DateClosed {get; set;}
    public decimal Balance {get; set;}
 
    // Constructor
    public Account()
    {
        // ...
    }
}
```

### Region

Les régions permettent de séparer le code. Les régions ne modifient strictement rien au code, elles sont juste présentes pour cacher facilement des morceaux de code pour faciliter la lecture. On peut les développer ou rétracter avec le raccourci suivant:
Ctrl + M (deux fois de suite)

Evitez de mettre des régions dans les méthodes.

**Par exemple:**

```
#region Commands
 
#region Declaration
 
public RelayCommand MaCommande {get; set;}
 
#endregion
 
#region Execution
 
private void MaCommande_Execute()
{
 
}
 
#endregion
 
#endregion
 
#region WS calls
 
#endregion
```
	

## Projet Windows

Un projet classique Backelite s'organise ainsi : 

+ YourProjectName
+ - Interfaces
+ - Misc
+ - + Converters
+ - + Extensions
+ - + Helpers
+ - Model
+ - Resources
+ - + images
+ - Services
+ - ViewModel
+ - Views







