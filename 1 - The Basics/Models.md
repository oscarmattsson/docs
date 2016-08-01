Models control the data source, they are used for collecting and issuing data, this could be a remote service, as XML, JSON or using a database to get and fetch records.

A Model is a class. The model needs to extend the parent Model, either the Core\Model or Database\Model (new Database API covered in the New Api's section).

The model should have a namespace of App\Models when located in the root of the app/Models directory.

````php 
namespace App\Models;

use Core\Model;

class Contacts extends Model 
{    
 
}
```

The parent model is very simple, it's the only role is to create an instance of the database class located in (**system/Helpers/Database.php**) once set the instance is available to all child models that extend the parent model.

````php 
namespace Core;

use Helpers\Database;

class Model  
{
    protected $db;

    public function __construct()
    {
        //connect to PDO here.
        $this->db = Database::get();

    }
}
```
Models can be placed in the root of the models folder. The namespace used in the model should reflect its file path. Classes directly in the models folder will have a namespace of models or if in a folder: namespace **App\Models\Classname**;

Methods inside a model are used for getting data and returning data back to the controller, a method should never echo data only return it, it's the controller that decides what is done with the data once it's returned.

The most common use of a model is for performing database actions, here is a quick example:

````php
public function getContacts()
{
    return $this->db->select('SELECT firstName, lastName FROM '.PREFIX.'contacts');
}
```

This is a very simple database query **$this->db** is available from the parent model inside the **$db** class holds methods for selecting, inserting, updating and deleting records from a MySQL database using PDO more on this topic in the section [[Database]].