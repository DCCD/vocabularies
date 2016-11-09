# Mapping Notes
**Peter Brewer**

 The tridabase-mapping.csv table can be used as a mapping between the original DCCD object/element type
 dictionary with the new DCCD/ARIADNE controlled vocabulary.  The list is based upon the 
 dictionary used in TRiDaBASE and therefore differs a litte from the Excel multi-lingual dictionary
 placed on the DCCD VKC. 

 Most records in DCCD should be matched using the normalid, term and lang values in this table 
 and be mapped to the DCCD/ARIADNE controlled vocabulary using the conceptuuid.

 Records in this list without a conceptuuid mapping are unused in the DCCD and have been determined to be
 superfluous 

 DCCD does allow for non-standardised terms too.  Where possible these have been accommodated here.
 Such terms will not have a normalid and thus should be matched using a simple text match with the term
 field. There will no doubt be a handful of terms in the DCCD that require manual mapping to the new 
 vocabulary.
 
 Please note that due to the way vocabularies are structured in TRiDaBASE the table contains terms from 
 all TRiDaS dictionaries, not just the object/element types that we are interested in here.
 
## Using the mapping
 
Here is an example of TRiDaS XML code from DCCD (originating from TRiDaBASE:
 ```xml
  <tridas:type lang="en" normal="Military camp" normalId="367" normalStd="DCCD">Legerplaats</tridas:type>
 ```
 
In this example the term has been standardised into English but with a Dutch translation in the tag.  As per the TRiDaS standard, the value of the tag is there for convenience and should be ignored.  It's the *lang, normal, normalId,* and *NormalStd* attributes of the tag that are important.  
 
Doing a lookup of *lang="en" normalId="367"* and *term="Military camp"* in the tridabase-mapping.csv table gives us a *conceptuuid* of {A7A268AC-6B80-11E5-AB6A-8BD9B38F7F29}
 
This UUID can be used to lookup the correct concept in the full SKOS vocabulary:
 
 ```skos
 dccd:a7a268ac-6b80-11e5-ab6a-8bd9b38f7f29 rdf:type skos:Concept;
  skos:scopeNote """In contemporary military contexts, use for groups of huts, tents, or other shelters set up temporarily for troops; in historical contexts use for permanent or semipermanent places of encampment; use 'military bases' for contemporary permanent installations."""@en;
  skos:exactMatch aat:300000476;
  skos:prefLabel "militärlager"@de;
  skos:prefLabel "military camp"@en;
  skos:prefLabel "camp militaire"@fr;
  skos:prefLabel "legerplaats"@nl;
skos:inScheme dccd:objectElementType.
```

Using the *prefLabel* of the language originally used (in this case en), and the original tag value for convenience (in this case Legerplaats) we can then create a new TRiDaS tag as follows:

```xml
  <tridas:type lang="en" 
    normal="military camp" 
    normalId="a7a268ac-6b80-11e5-ab6a-8bd9b38f7f29" 
    normalStd="tridas.org/vocabularies/objectelement.type.ttl">Legerplaats</tridas:type>
```  
The *normalStd* field should contain an identifier for the vocabulary being used.  I'm suggesting here using the URI of the Turtle file on the tridas.org server, but I'm open to other suggestions.  Upgrading the tag structure to full Linked Open Data should be on the agenda for the next iteration of TRiDaS.
  


 
 
