

# Backelite - Guide de Style Objective-C

## Sources 

* [Le langage de Programmation Objective-C](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Les Bases Fondamentales de Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Conseils Généraux de Codage pour Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Guide de Programmation pour App iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)
* [Apple-NamingMethods](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)
* [Apple-MemoryMgmt](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)
* [NYTimes - Guide de Style Objective-C](https://github.com/NYTimes/objective-c-style-guide)
* [github - objective-c-style-guideC](https://github.com/github/objective-c-style-guide)
* [Raywenderlich - objective-c-style-guideC](https://github.com/raywenderlich/objective-c-style-guide)
* [Google - objcguide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)

## Table des matières

* [Language](#language)
* [Nommage](#nommage)
* [Méthodes](#methodes)
* [Organisation du code](#organisation-du-code)
* [Variables](#variables)
* * Libellés (Literals)
* [Init & Dealloc](#init-et-dealloc)
* [Notation pointée](#notation-pointée)
* [Espacement](#espacement)
* [Conditions](#conditions)
* [Opérateur ternaire](#opérateur-ternaire)
* [Constantes](#constantes)
* [Types énumérés](#types-énumérés)
* [Masques de bits](#masques-de-bits)
* [Booléens](#booléens)
* [Propriétés privées](#propriétés-privées)
* [Literals](#literals)
* [Gestion des erreurs](#gestion-des-erreurs)
* [Fonctions CGRect](#fonctions-cgrect)
* [Projet Xcode](#projet-xcode)


## Language

US English should be used.

**Par exemple:**
```
UIColor *myColor = [UIColor whiteColor];
```

**Non pas:**
```
UIColor *myColour = [UIColor whiteColor];
```

## Nommage

La convention de nommage Apple devrait être suivie quand possible, surtout en ce qui concerne les [règles de management de mémoire](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

* Il est mieux d'utiliser des noms descriptifs, et longs si nécessaire, pour les méthodes et variables.

**Par exemple:**
```
UIButton *settingsButton;
```

**Non pas**
```
UIButton *setBut;
```

* Un préfixe de trois lettres (par ex. `JMO`) doit toujours être utilisé pour le nom des classes et constantes, mais peut être omis pour le nom des entités dans Core Data. Les constantes doivent adopter la convention camelCase avec tous les mots qui commencent avec une lettre capitale, précédées du nom de la classe dans laquelle ils sont déclarés pour la clarté.

**Par exemple:**

```
static const NSTimeInterval BkArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Non pas:**

```
static const NSTimeInterval fadetime = 1.7;
```

* Les properties et variables locales doivent adopter la convention camelCase avec le premier mot en minuscules.

* Les variables d'instance doivent adopter la convention camelCase avec le premier mot en miniscules, précédé par le préfixe  «&#8239;_&#8239;». Ceci est cohérent avec les variables d'instance synthetisées automatiquement par LLVM. **Si LLVM peut synthetiser la variable automatiquement, laissez-le faire.**

**Par exemple:**
```
@synthesize nomDeVariableDescriptif = _nomDeVariableDescriptif;
```

**Non pas:**
```
id nmvar;
```

## Méthodes

[Apple-NamingMethods](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)

 * Pour la signature d'une méthode, il doit y avoir un espace après le scope (symbole `-` ou `+`). Et il doit y avoir un espace entre les différents segments (paramètres) de la méthode.

**Par exemple:**:
```
- (void)setTextePourExemple:(NSString *)texte image:(UIImage *)image;
```

 * Longueur des méthodes, pas plus de 50 lignes. Se fixer cette limite permet de mieux découper les fonctionalités.

 * La signature des méthodes, il faut utiliser des mots clés pertinents pour décrire ce que va faire la méthodes et les arguments.
 
**Par exemple:**:

```
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (UIVIew *)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Non pas:**

```
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this, the "and" keyword is reserved.
```

* un seul `return` par méthode, pour faciliter les phases de debug et ne pas avoir à poser 10 breakpoints pour comprendre.

## Organisation du code

Utiliser les `#pragma mark -` pour organiser les méthodes et les regrouper en groupe fonctionnel. 

```
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - Actions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```


## Variables

Les variables doivent être nommées de la façon la plus descriptive possible. Une variable d'une seule lettre doit être évitée sauf pour une boucle `for`.

Les astérisques qui indiquent le pointeur sont plaçés **avant le nom** de la variable, par ex., `NSString *text` et **non** `NSString* text` ou `NSString * text`, sauf dans le cas de constantes (`NSString * const BkConstantString`).

La définition des propriétés doivent être utilisées à la place des variables d'instance quand c'est possible. L'accès direct aux variables d'instance doit être évité sauf pour les méthodes d'initialisation (`init`, `initWithCoder:`, etc…), la méthode `dealloc` et les accesseurs et mutateurs. Pour plus d'information sur l'utilisation de méthodes d'accès, les méthodes d'initialisation et dealloc, consultez [cet article](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Par exemple:**

```
@interface BKSection: NSObject
@property (nonatomic) NSString *headline;
@end
```

**Non pas:**

```
@interface BKSection : NSObject {
NSString *headline;
}
```

###variables locales
Quand on utlise des variables localement il faut penser à les initialiser.

```
NSError *error = nil;
NSUInteger count = 0;
```

### Libellés (Literals)
`NSString`, `NSDictionary`, `NSArray`, et `NSNumber` literals doivent être utilisés quand des instances immutables sont créées pour ces objets. Faites bien attention que la valeur `nil` ne soit pas passée aux literals `NSArray` et `NSDictionary`, parce que ça causerait un plantage.

**Par exemple:**

```
NSArray *names = @[@"Brian", @"Craig", @"Véronique"];
NSDictionary *productManagers = @{@"iOS" : @"Andrew", @"Android" : @"Kate"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Non pas:**

```
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Craig", @"Véronique", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Andrew", @"iOS", @"Kate", @"Android", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

### Types

* Utilisation des primitive d'Apple, NSInteger, NSUInteger plutôt que int, long. C'est une bonne idée pour avoir un comportement normal sur du 64 bits. De même, utilisation de  CGFloat plutôt que float.

* Respecter les types définis par Apple 

**Par exemple:**
```
CGRect rec = self.view.frame;
rec.x = 0.0f;
rec.x = 10.0f;
self.view.frame = rect;
```

**Non pas:**
```
CGRect rec = self.view.frame;
rec.x = 0.0;  	//ça c'est un double...
rec.x = 1;		//ça c'est un int...
self.view.frame = rect;
```

## Init et dealloc

La méthode `dealloc` doit être placée dans la partie "LifeCycle" de l'object (Cf organisation du code)
La méthode `init` doit être structurée comme ceci:

```
- (instancetype)init {
    self = [super init]; // ou appeler l'initialisateur désigné
    if (self) {
        // Initialisation particulière à cet object
    }
	
   return self;
}
```

Utiliser la macro `NS_DESIGNATED_INITIALIZER` pour documenter les init que vous voulez privilégier.

### et le new? 

Une petite méthode de fainéant. Déconseillé car la séparation des responsabilités allocation et initialisation n'est pas clair.
De plus, cela rend plus aléatoire l'`init` (le designated initializer) qui sera appellé.
Donc on va essayé de l'éviter.

## Notation pointée

La notation pointée doit **toujours** être utilisée pour lire ou modifier les propriétés. 
La notation crochée est préférable dans tous les autres cas.

**Par exemple:**
```
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Non pas:**
```
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

### Underscores ? ou pas ?

Privilégier l'usage de l'accesseurs en utilisant `self.` sauf pendant les méthodes d'init et de dealloc.

## Espacement

* L'indentation est de 4 espaces. N'indentez jamais avec des tabulations. Assurez-vous de régler cette préférence dans Xcode. (Pourquoi? car les tabulations peuvent être gérée différement par la configuration des utilisateurs et les outillages). J'ai par exemple beaucoup de soucis avec SourceTree.


**Par exemple:**

```
if (utilisateur.estHeureux) {
    //Faire quelque chose
	
} else {
    //Faire quelque chose d'autre
}
```

**Non pas (et oui ... ça semble pareil mais c'est une tabulation):**

```
if (utilisateur.estHeureux) {
	//Faire quelque chose
	
} else {
	//Faire quelque chose d'autre
}
```

## Conditions

* **TOUJOURS** utiliser des accolades

**Par exemple:**

```
if (!error) {
    return success;
}
```

**Non pas:**

```
if (!error) return success;
```

* L'accolade ouvrante des méthodes et structures de contrôle (`if`/`else`/`switch`/`while` etc.) est toujours sur la même ligne que la déclaration et l'accolade fermante sur sa propre ligne. 
* Si le mot clef est en rapport avec l'accolade fermante précédente on le met sur la même ligne (else, while du do…while)
* On met en espace entre le mot clef et la parenthese qui le suit, l'accolade qui le suit, l'accolade qui le précède
* Et un saut de ligne avant des conditions imbriquées (else if / else)

**Par exemple:**

```
if (!error) {
    return success;
	
} else {
    return failed;
}
```

**Non pas: (méga combo d'erreurs)**

```
if(!error){
    return success;
} else return failed;
```

* Mutliples conditions, placer l'opérateur en début de ligne.

**Par exemple:**

```
if (some_long_condition 
    && some_other_long_condition 
	|| some_completely_different_long_condition) {
}
```

* Mutliples conditions et conditions vraiment bien compliqué
**Par exemple:**

```
BOOL test1 = some_long_condition && some_other_long_condition;
BOOL test2 = some_other_long_condition && some_other_long_condition2;

if (test1 || test2) {
    ...
}
```

**Non pas:**

```
if (some_long_condition && some_other_long_condition || (some_other_long_condition && some_other_long_condition2) ||
    some_completely_different_long_condition) {
    
}
```

###Yoda style
Pour une écriture plus sure et éviter des erreurs humaines de saisies.

**Non pas:**
```
if (maValeur == 42) {
}
```
car il y'a un risque de frape et que l'on saisisse `maValeur = 42` ce qui n'engendre pas d'erreur


**Par exemple:**

```
if (42 == maValeur) {
}
```
On aura un gros warning si on écrit `42 = maValeur`

###Switch

* Utiliser des accolades

```
switch (something.state) {
    case 0: {
        // Something
        break;
    }
	
    case 2:
    case 3: {
        // Something
        break;
    }

    default: {
        // Something
        break;
    }
}

```

* Si l'ensemble des cas est couvert par le switch, ne pas mettre de default 

```
typedef NS_ENUM(NSInteger, BkState) {
    BkStateLoaded,
    BkStateLoading,
   BkStateInError
};

switch (BkState) {
    case BkStateLoaded: {
        // Something
        break;
    }

    case BkStateLoading: {
        // Something
        break;
    }

    case BkStateInError: {
        // Something
        break;
    }
}

```

### Opérateur ternaire

L'opérateur ternaire, `?` , doit seulement être utilisé s'il rend le code plus lisible ou propre. Il doit seulement évaluer une condition simple. Évaluer plusieurs conditions est généralement plus facile à comprendre avec une condition de type if, ou refactorisé avec des variables nommées.

**Par exemple:**
```
result = (a > b) ? x : y;
```

**Non pas:**
```
result = a > b ? x = c > d ? c : d : y;
```

## Constantes

Les constantes sont préférables aux literals in-line ou aux nombres, parce qu'elles peuvent être facilement reproduites de variables utilisés souvent et parce qu'elles peuvent être changées facilement sans avoir besoin de faire une recherche. 

### Constantes interne à une classe

**Par exemple:**

```
NSString * const BkActionNotification = @"Backelite notification";
const CGFloat BkImageThumbnailHeight = 50.0f;
```

**Non pas:**

```
#define CompanyName @"Backelite"

#define thumbnailHeight 2
```

### Constantes public

On peut rendre nos constantes public, en les exposant dans le .h.
On utiliser `static` pour s'assurer la constante ne sera pas dupliquée et que c'est bien l'instance unique qui est utilisée.

```
FOUNDATION_EXPORT static NSString * const BkActionNotification";
FOUNDATION_EXPORT static const CGFloat BkImageThumbnailHeight;
```

## Types énumérés

Pour l'utilisation d' `enum`, il est recommendé de choisir le type fixe spécifié avec un «&#8239;_&#8239;» parce qu'il est de type fort et pour bénéficier de la complétion de code. Le SDK inclus un macro pour faciliter et encourager l'utilisation de type fixe et souligné — `NS_ENUM()`

**Exemple:**

```
typedef NS_ENUM(NSInteger, BkState) {
    BkStateLoaded,
    BkStateLoading,
    BkStateInError
};
```

## Masques de bits

Quand vous travaillez avec des masques de bits, utilisez le macro `NS_OPTIONS`.
Pour pouvoir combiner 

**Exemple:**

```
typedef NS_OPTIONS(NSUInteger, BkState) {
    BkStateInError					= 1 << 0,
    BkStateInErrorWSFailed	   	    = 1 << 1,
    BkStateInErrorNetwork  			= 1 << 2
};
```

## Booléens

Puisque `nil` est retourné comme `NO` il n'est pas nécessaire de le comparer dans une condition. 
Ne comparez jamais quelque chose avec `YES`, parce que `YES` est défini comme 1 et un `BOOL` peut aller jusqu'à 8 bits.

Ce style permet une plus grande cohérence entre les différents fichiers et une meilleure clarté visuelle.

**Par exemple:**

```
if (!unObject) {
}
```

**Non pas:**

```
if (unObject == nil) {
}
```

Cependant, il est quand même plus correct d'avoir
```
if (monPointeur != nil) {
}
```

-----

**Pour un `BOOL`, voici deux exemples:**

```
if (estSuper)
if (!unObject.boolValue)
```

**Non pas:**

```
if (estSuper == YES) // Ne faites pas ça
if (unObject.boolValue == NO)
```

-----

Si le nom d'une propriété `BOOL` est exprimée comme un adjectif, la propriété peut omettre le prefixe «&#8239;is&#8239;» mais doit specifier un nom conventionel pour l'accesseur get, par exemple:

```
@property (assign, getter=isEditable) BOOL editable;
```
Voyez le document et exemple pris de [Conseils Généraux de Nommage Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).



## Propriétés privées

Les propriétés privées doivent être déclarées dans l'extension de la classe dans le fichier d'implémentation (la catégorie privée)

**Par exemple:**

```
@interface BkView ()

@property (nonatomic) NSUInteger *bkTag;
@property (nonatomic) BOOL bkViewIsLoaded;

@end
```


## Literals (On en parle?)

 * comment les présenter ?
 * Risque de crash si des valeurs sont vides?


## Gestion des erreurs

Quand une méthode renvoie un paramètre d'erreur par référence, continuez l'exécution du programme sur la valeur returnée, et non sur la variable erreur.

**Par exemple:**

```
NSError *error;
if (![self faireQuelqueChoseAvecRecuperationErreur:&error]) {
// Gérer l'erreur
// OUI, faireQuelqueChoseAvecRecuperationErreur doit retourner un BOOL pour dire si oui ou non, le résultat est considéré comme ok
}
```

**Non pas:**

```
NSError *error;
[self FaireQuelqueChoseAvecErreur:&error];
if (error) {
// Gérer l'erreur
}
```

Certaines APIs d'Apple renvoient des valeurs de données poubeille pour un paramètre erreur (si non-NULL), donc continuer l'exécution du programme sur l'erreur peut créer des faux négatifs (et par la suite un plantage).

## Fonctions `CGRect`

En accédant à `x`, `y`, `width`, ou `height` d'un `CGRect`, utilisez toujours les [fonctions `CGGeometry`](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) au lieu de l'accès direct au membre struct. Extrait de la référence Apple pour `CGGeometry`:

> Toutes les fonctions décrites dans cette référence qui prendre les structures de data CGRect comme donnée standardise implicitement ces rectangles avant de calculer leurs résultats. Pour cette raison, votre application devrait éviter de lire et écrire directement la donnée sauvegardée dans la structure de données CGRect. À la place, utilisez les fonctions décrites ici pour manipuler les rectangles et pour recupérer leurs caractériques.

**Par exemple:**

```
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Non pas:**

```
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Projet Xcode

Le choix que nous faisons sur nos projets est une architecture logique.
Une grande partie des classes sont dans le répertoire /Classes et le projet Xcode présente l'architecture logique.

Un projet classique Backelite s'organise ainsi : 

+ YourProjectName
+ - Generic
+ - + Categories
+ - + View
+ - + View Controller
+ - Metier
+ - + FormValidator
+ - + MetierHelper
+ - Model
+ - + BusinessModels
+ - + CityGuide
+ - + Helpers
+ - UI
+ - WS
+ Pods
+ - Lot of pods





