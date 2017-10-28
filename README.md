# Abstrac Class

__Abstract Class__ adalah sebuah class yang tidak bisa di-instansiasi (tidak bisa dibuat menjadi objek) dan berperan sebagai _kerangka dasar_. Abstract class digunakan di dalam inheritance (pewarisan class) untuk ‘memaksakan’ implementasi method yang sama bagi seluruh class yang diturunkan dari abstract class. Abstract class digunakan untuk membuat struktur logika penurunan di dalam pemrograman objek.bagi class turunannya. Di dalam abstract class umumnya akan memiliki abstract method.

### Aturan-aturan pada Abstrac Class dan Abstrac Method

1. Cara pembuatan abstract class dan abstract method
Cara membuatnya yaitu harus didahului dengan keyword `abstract` sebelum class dan method.

```php
abstract class User
{
    abstract protected function showName();
}
```

2. Abstract class tidak bisa dijadikan object

3. Jika dalam sebuah class terdapat abstract method maka class tersebut harus menjadi abstract class.

__SALAH__
```php
class User
{
    abstract protected function showName();
}
```
__BENAR__
```php
abstract class User
{
    abstract protected function showName();
}
```

4. Abstract method hanya boleh signature
Artinya abstract method tidak boleh memiliki body, yaitu hanya berupa deklarasi saja dan tidak memiliki isi.

__SALAH__
```php
abstract class User
{
    abstract protected function showName();
    {
        
    }
}
```
__BENAR__
```php
abstract class User
{
    abstract protected function showName();
}
```

5. Semua class turunan harus mengimplementasikan semua abstract method dari parent class
Hanya abstract method yang diturunkan, sedangkan untuk regular method tidak diturunkan.
```php
abstract class User
{
    abstract protected function showName();
}

class Admin extends User
{
    public function showName()
    {
        return "Dhezign";
    }
}
```

6. Semua method turunan dari abstract method harus didefinisikan dengan tingkat visibilitas yang sama atau lebih rendah.
Urutan tingkatan akses level dari tinggi ke rendah adalah 1. private, 2. protected 3. public.

```php
abstract class User
{
    abstract protected function showName();
}

class Admin extends User
{
    public function showName()
    {
        return "Dhezign";
    }
}
```

7. Abstract class boleh memiliki property dan method regular
```php
// abstract class
abstract class User
{
    // regular property
    protected $address = 'Surabaya';

    // abstract method
    abstract protected function showName();
    abstract public function showGreeting($greeting);
    
    // regular method
    public function showBio()
    {
        return "Hi, my name is " . $this->showName() . " from " . $this->address;
    }
}
```

8. Abstract class boleh memiliki static method
```php
// abstract class
abstract class User
{
    // abstract method
    abstract protected function showName();
    
    // static method
    public static function showHi()
    {
        return "Hi, this is static method";
    }
}

// panggil static method dari abstract class
echo User::showHi();
```

9. Semua method turunan dari abstract method harus mengikuti signature
Misal dalam signature disertai required argument maka method dalam child class harus memiliki required argument tersebut.

```php

abstract class User
{
    abstract protected function showName();
    
    abstract public function showGreeting($greeting);
}

class Admin extends User
{
    public function showName()
    {
        return "Dhezign";
    }

    public function showGreeting($greeting)
    {
        return "My name is " . $this->showName();
    }
}
```

# Anonymous class

Anonymous Class adalah class anonim alias tanpa nama. Anonymous class berguna jika kita mendadak membutuhkan object sederhana tanpa ingin ribet didahului pembuatan regular class seperti biasanya.

#### Contoh Penggunaan dengan anonymous class

```php
class User 
{

    private $name;

    public function setName($name) 
  {
        $this->name = $name;
  }

    public function getName() 
  {
        return $this->name;
  }

}

/*
* instansiasi
*/
$user1 = new User;

/*
* anonymous class
*/
$user1->setName(new class 
{

    public function show($name) 
    {

        return "Nama: " . $name;
    }

});

echo $user1->getName()->show("Dewi");
```

#### Contoh tidak menggunakan anonymous function

```php
class User 
{

    private $name;

    public function setName($name) 
    {
        $this->name = $name;
    }

    public function getName() 
    {
        return $this->name;
    }

}

/*
* membuat class baru
*/
class ShowName 
{

    public function show($name) 
    {
        return "Nama: " . $name;
    }

}

/*
* instansiasi
*/
$show = new ShowName();
$user = new User;

/*
* set name dari object
*/
$user->setName($show);

echo $user->getName()->show("Dewi");

Jadi, perbedaan dari penggunaan anonymous function dan tidak adalah jika menggunakan anonymous function kita tidak perlu lagi membuat object baru dan script nya juga lebih simple.

Sebenarnya anonymous class memiliki nama, ini bisa dibuktikan dengan menjalankan sebuah function yang digunakan untuk mengambil nama class dari sebuah object: `get_class(object)`

```php
echo get_class(new class {});
```

### Anonymous class bertindak seperti class regular :

**a. Boleh diberi argumen, meng-extend class lain, meng-implement interface dan menggunakan trait.**

#### Contoh
```php
class ThisClass {}
interface ThisInterface {}
trait ThisTrait {}

class User 
{

    private $name;

    public function setName($name) 
    {
        $this->name = $name;
    }

    public function getName() 
    {
        return $this->name;
    }

}

/*
* instansiasi
*/
$user1 = new User;

/*
* anonymous class
*/

$user1->setName(new class('Marta') extends ThisClass implements ThisInterface {
  
  /*
  * menggunakan ThisTrait
  */
    use ThisTrait;

    private $lastName;

    public function __construct($lastName)
    {
        
        $this->lastName = $lastName;
    }

    public function show($name) 
    {

        return "Nama : " . $name . " " . $this->lastName;
    }

});

echo $user1->getName()->show("Dewi");
```

**b. Tidak boleh akses private dan protected**

Ketika anonymous class dibungkus oleh class lain, walaupun dibungkus oleh class lain tetap saja anonymous class tidak diperbolehkan mengakses private dan protected method atau property yang berasal dari class yang membungkusnya.

#### Contoh

```php
class Balon
{
    private $warna = 'merah ';

    protected $bahan = 'karet ';

    public function kegunaan() 
    {
      return "Ulang Tahun";
    }

    public function diPakai() 
    {

        /* 
        * jika ingin menggunakan protected property
        * dari class Balon maka harus
        * meng-extend class Balon
        * walaupun anonymous class dibungkus / berada dalam class Balon
        */
        return new class($this->warna) extends Balon 
        {

            private $warnanya;

            public function __construct($warna)
            {
                $this->warnanya = $warna;
            }

            public function guna()
            {
                /*
                * $this->bahan berasal dari class Balon
                * boleh diakeses setelah meng-extend class Balon
                */
                return "Balon " . $this->bahan . "warna " . $this->warnanya . "dipakai untuk " . $this->kegunaan() . " Adik";
            }
        };
    }
}

$bungkus = new Balon();

echo $bungkus->diPakai()->guna();
```
