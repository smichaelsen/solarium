This facet type allows you to count the number of occurrences of a term in a specific field.

Options
-------

The options below can be set as query option values, but also by using the set/get methods. See the API docs for all available methods.

Only the facet-type specific options are listed. See [Facetset component](V3:Facetset_component "wikilink") for the option shared by all facet types.

| Name               | Type    | Default value | Description                                                                                                                                          |
|--------------------|---------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| field              | string  | null          | The index field for the facet.                                                                                                                       |
| prefix             | string  | null          | Limit the terms on which to facet to those starting with the given prefix. This does not limit the query, only the facets.                           |
| contains           | string  | null          | Limit the terms on which to facet to those containing the given substring. This does not limit the query, only the facets. Available since Solr 5.1. |
| containsignorecase | boolean | null          | If 'contains' is used, causes case to be ignored when matching the given substring against candidate facet terms.                                    |
| sort               | string  | null          | Sort order (sorted by count). Use one of the class constants.                                                                                        |
| limit              | int     | null          | Limit the facet counts.                                                                                                                              |
| offset             | int     | null          | Show facet count starting from this offset.                                                                                                          |
| mincount           | int     | null          | Minimal term count to be included in facet count results.                                                                                            |
| missing            | boolean | null          | Also make a count of all document that have no value for the facet field.                                                                            |
| method             | string  | null          | Use one of the class constants as value. See <https://solr.apache.org/guide/faceting.html#field-value-faceting-parameters> for details.              |
||

Example
-------

```php
<?php

require_once(__DIR__.'/init.php');
htmlHeader();

// create a client instance
$client = new Solarium\Client($adapter, $eventDispatcher, $config);

// get a select query instance
$query = $client->createSelect();

// get the facetset component
$facetSet = $query->getFacetSet();

// create a facet field instance and set options
$facetSet->createFacetField('stock')->setField('inStock');

// this executes the query and returns the result
$resultset = $client->select($query);

// display the total number of documents found by Solr
echo 'NumFound: '.$resultset->getNumFound();

// display facet counts
echo '<hr/>Facet counts for field "inStock":<br/>';
$facet = $resultset->getFacetSet()->getFacet('stock');
foreach ($facet as $value => $count) {
    echo $value . ' [' . $count . ']<br/>';
}

// show documents using the resultset iterator
foreach ($resultset as $document) {

    echo '<hr/><table>';
    echo '<tr><th>id</th><td>' . $document->id . '</td></tr>';
    echo '<tr><th>name</th><td>' . $document->name . '</td></tr>';
    echo '<tr><th>price</th><td>' . $document->price . '</td></tr>';
    echo '</table>';
}

htmlFooter();

```
