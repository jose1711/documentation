# Structured geocoding

With structured geocoding, you can search for the individual parts of a location. Structured geocoding is an option on the [`search` endpoint](search.md), where a query takes the form of https://search.mapzen.com/v1/search/structured.

For example, you want to find `30 West 26th Street, New York, NY`. With the geocoding parameter for `search`, you can only enter the entire location as one string, such as `text=30 West 26th Street, New York, NY`. However, with `search/structured`, you can specify that this location is composed of a street address, a locality, and a region.

```
{
  address: '30 West 26th Street',
  locality: 'New York',
  region: 'NY'
}
```

Structured geocoding can improve how the items in your query are parsed and interpreted in a search. An address such as `10 Park Place North Charleston South Carolina` could be viewed as a city name containing a directional (North Charleston in South Carolina), or as a street with a post-directional (10 Park Place North). By separating the components of the search input, you are reducing ambiguity in your query. This is also helpful because addresses and postal codes around the world are often formatted and ordered differently.

One common use for structured geocoding is for geocoding a list or table of addresses, where each column represents a different portion of a location.

| address | city | state | country |
| ------- | ---- | ----- | ------- |
| 1600 Pennsylvania Ave | Washington | DC | US |
| 10 Downing Street | London | | GB |
| 55 Rue du Faubourg Saint-Honoré | Paris | | FR |
| Bulevardul Geniului 1 | Bucharest | | Romania |

With a regular `search` query, you would need to concatenate these columns into one string, but `search/structured` allows you to define a query that maintains the individual fields for address, city, state, and country.

## Structured geocoding parameters

You can use structured geocoding to search for the following parameters:

* address
* neighbourhood
* borough
* locality
* county
* region
* postalcode
* country

Note that the other [`search` parameters](search.md/#available-search-parameters) can also be combined with these, allowing you to filter and prioritize your results.

### address

The `address` parameter can contain a full address with house number or only a street name.

_Examples_

* [201 Spear Street](http://search.mapzen.com/v1/search/structured?address=201+Spear+Street&locality=San+Francisco&region=CA)
* [Rue de Rivoli](http://search.mapzen.com/v1/search/structured?address=Rue+de+Rivoli&locality=Paris&region=France)
* [Přílucká 1](http://search.mapzen.com/v1/search/structured?address=1+Přílucká&locality=Želechovice+nad+Dřevnicí)

### neighbourhood

[Neighbourhoods](https://whosonfirst.mapzen.com/spelunker/placetypes/neighbourhood/) are vernacular geographic entities that may not necessarily be official administrative divisions but are important nonetheless.  

_Examples_

* [Notting Hill](http://search.mapzen.com/v1/search/structured?neighbourhood=Notting+Hill&locality=London) in London
* [Flatiron District](http://search.mapzen.com/v1/search/structured?neighbourhood=Flatiron+District&borough=Manhattan) in Manhattan
* [Le Marais](http://search.mapzen.com/v1/search/structured?neighbourhood=Le+Marais&locality=Paris) in Paris

### borough

[Boroughs](https://whosonfirst.mapzen.com/spelunker/placetypes/borough/) are mostly known in the context of New York City, even though they may exist in other cities, such as [Mexico City](https://whosonfirst.mapzen.com/spelunker/id/857683023/descendants/?exclude=nullisland&placetype=borough).

_Examples_

* [Manhattan](http://search.mapzen.com/v1/search/structured?borough=Manhattan&locality=New+York)
* [Iztapalapa](http://search.mapzen.com/v1/search/structured?borough=Iztapalapa&locality=Mexico+City)

A structured geocoding request for `/v1/search/structured?locality=Manhattan&region=NY`, returns boroughs along with localities.  

### locality

[Localities](https://whosonfirst.mapzen.com/spelunker/placetypes/locality/) are equivalent to what are commonly referred to as cities.  

_Examples_

* [Bangkok](http://search.mapzen.com/v1/search/structured?locality=Bangkok&country=Thailand)
* [Caracas](http://search.mapzen.com/v1/search/structured?locality=Caracas&country=Venezuela)
* [Truth or Consequences](http://search.mapzen.com/v1/search/structured?locality=Truth+or+Consequences&region=NM) in New Mexico

### county

[Counties](https://whosonfirst.mapzen.com/spelunker/placetypes/county/) are administrative divisions between localities and regions.  

_Examples_

* [Bucks](http://search.mapzen.com/v1/search/structured?county=Bucks&region=PA) in Pennsylvania
* [Maui](http://search.mapzen.com/v1/search/structured?county=Maui&region=HI)
* [Alb-Donau-Kreis](http://search.mapzen.com/v1/search/structured?county=Alb-Donau-Kreis&country=DEU) in Germany

Counties are not as commonly used in geocoding as localities, but can be useful when attempting to disambiguate between localities. For instance, there are three cities named Red Lion in Pennsylvania but only one in each of three counties. Specifying a county disambiguates this list to a single result.  

### region

[Regions](https://whosonfirst.mapzen.com/spelunker/placetypes/region/) are normally the first-level administrative divisions within countries, analogous to states and provinces in the [United States](https://whosonfirst.mapzen.com/spelunker/id/85633793/descendants/?exclude=nullisland&placetype=region) and [Canada](https://whosonfirst.mapzen.com/spelunker/id/85633041/descendants/?exclude=nullisland&placetype=region), respectively, though most other countries contain regions as well.  

_Examples_

* [Delaware](http://search.mapzen.com/v1/search/structured?region=Delaware)
* [Ontario](http://search.mapzen.com/v1/search/structured?region=Ontario)
* [Ardennes](http://search.mapzen.com/v1/search/structured?region=Ardennes)

Regions in the United States have [common abbreviations](https://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations), such as PA for [Pennsylvania](https://whosonfirst.mapzen.com/spelunker/id/85688481/) and NM for [New Mexico](https://whosonfirst.mapzen.com/spelunker/id/85688493/).  The `region` parameter can be a full name or abbreviation, so specifying `/v1/search/structured?region=NM` is functionality equivalent to `/v1/search/structured?region=New Mexico`.  

### postalcode

[Postal codes](https://whosonfirst.mapzen.com/spelunker/placetypes/postalcode/) are used to aid in sorting mail with the format dictated by an administrative division, which is almost always countries.  Among other reasons, postal codes are unique within a country so they are useful in geocoding as a shorthand for a fairly granular geographical location.

_Examples_

* [90210](https://whosonfirst.mapzen.com/spelunker/id/554783991/)
* [CV23 9SL](https://whosonfirst.mapzen.com/spelunker/id/454261459/)
* [5439171](https://whosonfirst.mapzen.com/spelunker/id/538904173/)

Keep in mind that you can search for `postalcode` exclusively. So requests like [/v1/search/structured?postalcode=87801](http://search.mapzen.com//v1/search/structured?postalcode=87801) will return matching postalcode records.

### country

[Countries](https://whosonfirst.mapzen.com/spelunker/placetypes/country/) are the highest-level administrative divisions supported in a search. In addition to full names, countries have common [two-](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) and [three-letter](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3) abbreviations that are also supported values for the `country` parameter.  

_Examples_

* [Liechtenstein](http://search.mapzen.com/v1/search/structured?country=Liechtenstein)
* [CMR](http://search.mapzen.com/v1/search/structured?country=CMR) ([Cameroon](https://whosonfirst.mapzen.com/spelunker/id/85632245/))
* [Bermuda](http://search.mapzen.com/v1/search/structured?country=Bermuda)

## Who's On First layer mappings reference

The [Who's on First](https://whosonfirst.mapzen.com/) gazetteer is one of the datasets used as a source of search data. Use this table is a reference between the parameters for structured geocoding and the [types of places in Who's on First](https://whosonfirst.mapzen.com/placetypes/).

| Structured geocoding parameter | Who's on First placetype |
| -------------------- | ------------------------- |
| `neighbourhood`        | [neighbourhood](https://whosonfirst.mapzen.com/spelunker/placetypes/neighbourhood/)             |
| `borough`              | [borough](https://whosonfirst.mapzen.com/spelunker/placetypes/borough/)                   |
| `locality`             | [locality](https://whosonfirst.mapzen.com/spelunker/placetypes/locality/), [localadmin](https://whosonfirst.mapzen.com/spelunker/placetypes/localadmin/) (and [borough](https://whosonfirst.mapzen.com/spelunker/placetypes/borough/) if `borough` parameter is not supplied)      |
| `county`               | [county](https://whosonfirst.mapzen.com/spelunker/placetypes/county/), [macrocounty](https://whosonfirst.mapzen.com/spelunker/placetypes/macrocounty/)       |
| `region`               | [region](https://whosonfirst.mapzen.com/spelunker/placetypes/region/), [macroregion](https://whosonfirst.mapzen.com/spelunker/placetypes/macroregion/)       |
| `country`              | [dependency](https://whosonfirst.mapzen.com/spelunker/placetypes/dependency/), [country](https://whosonfirst.mapzen.com/spelunker/placetypes/country/)       |

For example, [Peach Bottom, Pennsylvania](https://whosonfirst.mapzen.com/spelunker/id/404487863/) is only a `localadmin` place type and not a `locality` in Who's on First. For simplicity, if a structured geocoding request specifies `locality=Peach+Bottom&region=Pennsylvania`, then `Peach Bottom` in both the `locality` and `localadmin` layers are searched.