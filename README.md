silverstripe-pickerfield
========================

SilverStripe 3 GridField based management of has_one , has_many , and many_many relationship selection.

Supports search filtration; aka limiting the available selections. 


## Requirements
* SilverStripe >= 3.1
* Andrew Short's GridFieldExtensions [ https://github.com/ajshort/silverstripe-gridfieldextensions ]

## Why?

1. Because we needed a consistent interface to manage relationship selections in an efficient* manner.
2. GridField doesn't appear to natively support has_one relationships.
3. Because AndrewShort's ItemSetFields don't work in SilverStripe 3

Thanks to the great work of the SilverStripe team and Andrew Short's GridFieldExtensions, this was relatively easy 
to implement. Please buy them beers or tea.

\* by efficient we needed ajax + pagination, as we couldn't load all records into a dropdown list for instance.

## Usage Overview

Screenshots;
###overview
![overview](https://github.com/briceburg/silverstripe-pickerfield/blob/master/docs/screenshots/pickerfield.png?raw=true)
###link button search [via GridFieldExtensions]
![link button search [via GridFieldExtensions]](http://github.com/briceburg/silverstripe-pickerfield/blob/master/docs/screenshots/add-existing-search.png?raw=true)


```php

/***********************
	Mock DataObject
************************/

class Dog extends DataObject {
	static $db = array(
		'Title'      => 'Varchar',
		'Gender'     => 'Enum("male,female")',
		// ....
	);
	
	static $has_one = array(
		'Breeder'    => 'Breeder'
		'Dam'        => 'Dog',
		'Sire'       => 'Dog',
		// ....
	);
	
	static $has_many = array(
		'Owners'     => 'Member',
		// ....
	);
	
// ....
}


/***********************
	Field Usage
************************/

// sortable field appropriate for selection of has_many and many_many objects
$field = new PickerField('Owners', 'Owners', $this->Owners(), 'Select Owner(s)', 'SortOrder');

// non-sortable version of the above
$field = new PickerField('Owners', 'Owners', $this->Owners());

// sortable field appropriate for the parent selection of a has_one relationship
$field = new HasOnePickerField($this, 'DamID', 'Selected Dam', $this->Dam(), 'Select a Dam');


// example of search filtration to limit selection to a desired subset.
///////////////////////////////////////////////////////////////////////
$fields->addFieldsToTab('Root.Pedigree', array(
	$sireField = new HasOnePickerField($this, 'SireID', 'Selected Sire', $this->Sire(), 'Select a Sire'),
	$damField  = new HasOnePickerField($this, 'DamID', 'Selected Dam', $this->Dam(), 'Select a Dam'),
	// ...
));

$damField->setSearchFilters(array('Gender' => 'female'));
$sireField->setSearchFilters(array('Gender' => 'male'));
///////////////////////////////////////////////////////////////////////

```


## Bugs

For support or questions, please use the GitHub provided issue tracker;
* https://github.com/briceburg/silverstripe-pickerfield/issues
