# MongoDB samenvatting
Dit is een Nederlands/ deels engelse korte samenvatting van het boek 'The little MongoDB boek'. Src= https://www.openmymind.net/mongodb.pdf
### Authors

* **R. Boudewijn** - [nxttx](https://github.com/nxttx)
* **Naam** - [username](https://github.com/username)

## Chapter one

$eq, 		equal

$lt, 		less than

$lte, 		less than or equal

$gt, 		greater than

$gte,		greather than or equal.

$ne, 		not equal  

$exists,	the presence or absence of a field

$in,		operator is used for matching one of several values, Mee gegeven als array 

$or & $and	kan meerdere dingen filteren

```
db.unicorns.find({gender: 'm',
weight: {$gt: 700}})
```
returned dus alle urnicorns die man zijn en een groter gewicht hebben dan 700.


```
db.unicorns.find({
vampires: {$exists: false}})
```
returned de unicorns die geen vampires fields hebben.

```
db.unicorns.find({
loves: {$in:['apple','orange']}})
```
This returns any unicorn who loves ‘apple’ or ‘orange

```
db.unicorns.find({gender: 'f',
$or: [{loves: 'apple'},
{weight: {$lt: 500}}]})
```
The above will return all female unicorns which either love apples or weigh less than 500 pounds.

## Chapter two

$set 		Gebruik dit keyword vor het 'setten' van data in een row.

$inc 		Operator is used to increment a field by a certain positive or negative amount. (dus met een tikje vergroten of verkleinen.)

$push 		Met push kan je een nieuwe string of int of etc toevoegen aan een array.


```
db.unicorns.update({name: 'Roooooodles'},
{$set: {weight: 590}})
```
Om een update te gebruiken moet je het keywoord ```$set``` gebruiken, hierdoor zorg je er voor dat je alleen weight aanpast in plaats van het hele object te verwijderen en alleen weight toevoegt. 

```
db.unicorns.update({name: 'Pilot'},
{$inc: {vampires: -2}})
```
Hierbij wordt ```$inc``` dus gebruikt om de de waarde groter te maken of kleiner te maken

```
db.unicorns.update({name: 'Aurora'},
{$push: {loves: 'sugar'}})
```
```$push``` wordt gebruikt om net als in js een object (int, string, char etc.) in een array te zetten 



Een van de voordelen van het gebruik van update is dat je gebruik kan maken van upsert. Dat houdt in dat wanneer je een object probeert te updaten maar het bestaat niet dat er dan een object wordt aangemaakt.  
Synatx:
```
db.hits.update({page: 'unicorns'},
{$inc: {hits: 1}}, {upsert:true});
db.hits.find();
```

Verder is het handig om te weten dat wanneer je een update zoals hier onder je alleen maar 1 regel update. 
```
db.unicorns.update({},
{$set: {vaccinated: true }});
db.unicorns.find({vaccinated: true});
```
Om dit te voorkomen kan je `{multi:true})` toevoegen. Je command zal er dan zo uit zien:

```
db.unicorns.update({},
{$set: {vaccinated: true }},
{multi:true});
db.unicorns.find({vaccinated: true});

```
Nu zullen alle 'rows' van unicorns 'vaccinated true' hebben.

## chapter three


We kunnen extra filteren door bij find een extra waarde mee tegeven. Bijvoorbeeld:
```
db.unicorns.find({}, {name: 1});
```
Bij dit voorbeeld worden alleen de 'colommen' name en _id gegeven. _id wordt altijd mee gegeven behalve als je in de find(,x) zegt `{_id : 0}`.
Deze functie kan je vergelijken met de sql: 
```sql
SELECT _id, name
FROM unicorns 
```

Er is ook een mogelijkheid om te sorteren.
```
db.unicorns.find().sort({weight: __x__})

```
Waarbij '__x__' 1(sql ASC) of -1(sql DESC) kan zijn. In het geval van 1 zullen cijfers van klein naar groot weer gegeven worden (bij -1 van groot naar klein). En voor teksten zal 1 van ABC en -1 ZYX


### START TODO - Weet niet of deze info 100% klopt. Dit onderwerp begreep ik zelf ook niet goed.{

Paging/cursors kunnen we gebruiken om bijvoorbeeld het derde resultaat te krijgen van een gewone find(). Bijvoorbeeld:
```
db.unicorns.find()
.sort({weight: -1})
.limit(2)
.skip(1)
```
Hierdoor krijg je een de derde zwaarste object van je database.

### } END TODO
We kunnen ook de hoeveelheid results tellen. Dit doe je met de `.count()` functie. Bijvoorbeeld:
``` 
db.unicorns.count({vampires: {$gt: 50}})
```
Hierdoor krijg je de hoeveelheid eenhoorns die boven de 50 vampieren hebben.


## Chapter four

Er zijn in mongodb geen mogelijkheden om joins te maken zoals bij sql. Maar het heeft wel een aantal andere dingen die kunnen helpen. Doordat je arrays als waarde kan mee geven kan je daardoor bijvoorbeeld meerdere managers geven aan werknemers. Waardoor een extra table onnodig is en je daardoor ook niet hoeft te joinen.

Je kan ook objecten in objecten (of een array met objecten in een object) zetten. Waardoor je bij het onderstaande object:
```
{
_id: ObjectId("4d85c7039ab0fd70a117d734"),
name: 'Ghanima',
family: {
	mother: 'Chani',
	father: 'Paul',
	brother: ObjectId("4d85c7039ab0fd70a117d730")
	}
}
```

De volgende find query kan uitvoeren:

```
db.employees.find({
'family.mother': 'Chani'})

```

Doordat joins niet mogelijk zijn wordt denormaliseren steeds vaker toegepast. Dit mag ook in MongoDB. Niet tot de extremen maar, je mag er mee experimenteren.

## Chapter five 
Aanrader om kompleet te lezen. Hier in wordt namelijk behandeld of en hoe mongoDB een goed alternatief is en de sterke en zwakke punten. 


> "<i>when people ask where does MongoDB sit with respect to the new
> data storage landscape?</i> the answer is simple: <b>right in the middle.<b>"

