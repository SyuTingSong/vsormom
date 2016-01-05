# vsormom

> Very Simple Object Relational Mapping over Medoo

## Features

* Based on Medoo TSD
* Predefined `CRUD` methods in Model
* Getter & Setter support
* JSON friendly

## Define Your Model Classes

    // Just inherit from the Model class
    class UserModel extends Model {
        // Declare default value for each field here
        protected $_properties = [
            'id' => null,
            'username' => '',
            'password' => '',
            'nick' => '',
            'balance' => 0.0,
            'is_blocked' => false,
            'create_time' => 0
        ];
        
        // Declare data type for each field here
        protected static $_meta = [
            'id' => 'int',
            'username' => 'str',
            'password' => 'str',
            'nick' => 'str',
            'balance' => 'float',
            'is_blocked' => 'bool',
            'create_time' => int
        ];
        
        // Override the `table_name` getter
        public function get_table_name() {
            return 'user';
        }
        
        // Exclude password from the object when json stringify
        // Include head_image into the object when json stringify
        protected $_exported = ['-password', '+head_image'];
        
        // A property getter
        public function get_head_image() {
            return $this->_new ?
                'http://www.example.com/anonymous.jpg' :
                ('http://www.example.com/' . $this->id % 10 . '.jpg');
        }
    }

## Init Medoo

    Model::setMedoo(new medoo([
        'database_type' => 'sqlite',
        'database_file' => '../all.db'
    ]));

## Use Model Object

### Create

    $user = Model::getInstance('User');
    $user->username = 'Joe';
    $user->password = 'abc';
    $user->nick = 'CamelCat';
    $user->create_time = time();
    $user->save();
    
    echo $user->id;

### Read

    $user = Model::getInstance('User')->one(['id' => 1]);

### List

    $users = Model::getInstance('User')->many([
        'username[~]' => 'th_'
    ]);

### Update

    $user = Model::getInstance('User')->one(['id' => 1]);
    if ($user instanceof UserModel) {
        $user->nick = 'Cool Tool';
        $user->save();
    }

### Delete

    Model::getInstance('User')->remove(1);

