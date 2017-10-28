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
abstract class User
{
    protected $address = 'Semarang';

    abstract protected function showName();
    abstract public function showGreeting($greeting);

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

# Trait

**trait** Trait adalah fitur baru pada PHP 5.4. Dengan trait kita dimungkinkan untuk menggunakan ulang sebuah kode (re-use). Anggap saja trait adalah copy-paste kode program yang dilakukan melalui bahasa pemograman. Kode program di dalam sebuah trait akan di-paste ke class lain yang memakainya.

***Contoh Penggunaan***
```PHP
trait share
{
    public function share()
    {
        return "Hello World!";
    }
}
class copy
}
    use share;
}
$copy = new copy;
echo $copy->share();
```
Keluaran : Hello World!

### Keuntungan menggunakan Trait
- Keuntungan menggunakan trait adalah mengurangi duplikasi kode dan juga mencegah class inheritance yang rumit yang tidak masuk akal dalam konteks aplikasi. Jadi kamu bisa membuat sebuah trait yang jelas dan ringkas kemudian memanggilnya dalam sebuah class.
- Pada saat sebuah class perlu mengimplementasikan lebih dari satu superclass. PHP tidak mendukung multiple inheritance, maka dari itu kita menggunakan trait.

### Kelemahan menggunakan Trait
 - Traits bisa membuat sebuah class memiliki tugas yang terlalu banyak, dengan kemudahan memasukkan fungsi-fungsi ke dalam sebuah class lewat Trait, maka besar kemungkinan fungsi class tersebut sangat mudah melebar kemana-mana, dimana seharusnya sebuah class hanya memiliki 1 fungsi utama sesuai dengan **single responsibility principle**(*“every class should have a single responsibility, and that responsibility should be entirely encapsulated by the class.”*).
 - Tidak bisa melihat semua method yang berada di class tersebut, jadi agak sulit jika terdapat error karena nama method yang sama ataupun logic yang sama dalam sebuah fungsi.
