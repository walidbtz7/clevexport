## Motivation
The goal of this Laravel package is to execute the exportation to **EXCEL** large data/records that may cause in the crash of the server or a timeout. The idea is to divide the process in sub operations managable and easy to perform.

## Installation
````
compose require einani/clevexport
````
After the installation make sure to publish the assets
````
php artisan vendor:publish --provider="Leyton\ClevExportServiceProvider"
````

You will find the ````config/clevexport.php```` file containing all the configurations needed.
````php
return [
    // array of listeners that will be executed once the execution is done
    'listeners' => [], 
    
    // if you want to stock the user who started the export
    'with_owner' => true,
    
    // the guard    
    'guard' => 'web',
    
    // the foreign key name in the exports table
    'owner_id' => 'user_id',
    
    // The Authenticable class
    'owner_class' => \App\Models\User:class,
    
    // Number of chunks
    'chunks' => 10,
];
````


Then you can run your migration

````
php artisan migrate
````

## Usage

The ``QueryFinder`` should be provided with an instance of an object that implements the ``IseExportable`` Interface

````php

 $dossierExporter = QueryFinder::getInstance($this->defaultDossierService, $this->transformer);

 PreparingExportJob::dispatch($dossierExporter, $request->all())->delay(now()->addSecond());
````

A second Parameter is optional and if is provided it should implements the ``ShouldHandleResult`` It is where you can perform extra work on the results and provide the headers in a ``ExportTransformed`` container
If the second parameter is not provided then the headers will be the column names selected from the query.

