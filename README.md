# ke-yd1
A Python3 encryption algorithm using secure key exchange based on Diffie Helman.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

```
Python3 +
```

### Installing

A step by step series of examples that tell you how to get a development env running

First downlad the gihub project and include the "keyd" folder in your project.
To use the library, import the keyd file from the keyd folder

```
from keyd import keyd
```

Thats it!

## Usage

How to get started using keyd

Note: currently keyd1 only supports a-z & 0-9 characters, more coming soon.

### Creating a user ###
To encrypt and decrypt messages, you need to make a keyd_node object. keyd_node objects act as a user, and are intended to only be used for communication with ONE other party, to communicate with another party, create a new keyd_node object.
 ```
user1 = keyd.keyd_node()
 ```
you can specify the private key, base, modulus, epoch scale and pair value
 ```
user1 = keyd.keyd_node(private_key=1, base=123, modulus=123, epoch_scale=2, pair_value=123456789)
 ```
if the private key isn't specified, one will randomly be generated, all other values will use built in defaults

### Setting up an exchange

To send a message from user1 to user2, a common exchange key is required, this is used for both parties to encrypt and decrypt messages. Note, both parties MUST have the exact same base, modulus, epoch scale, and pair value for exchanges otherwise it wont work (a different epoch value for exchange keys wont matter but encryption and decyryption wont work).

```
user1.init_exchange(user2.get_public_key()) # initialise an exchange, from user 1 to user 2
user2.init_exchange(user1.get_public_key()) # initialise an exchange, from user 2 to user 1
```
Now both parties have a common exchange key, assuming they both initially had the same base, modulus and pair value. Init exchange adds the public key specified and an exchange key to a dictionary in the object, so the user doesnt have to worry about managing exchange keys.

### Encrypting and Decrypting

To encrypt a message

```
message="hello there"
encrypted=user1.encrypt(message,user2.get_public_key()) # user 1 encrypts a message to send to user 2
```
The encrypted message will look like something along the lines of 
```
25723811111143548941-5257238121231
```
We have to specify the destination public key (this will usually be known and this getter function is only for demonstration purposes) for it to lookup in the dictionary for the corresponding exchange key.

Decrypting is just as simple
```
decrypted=user2.decrypt(encrypted,user1.get_public_key()) # user 2 decrypts a message recieved from user 1
```
### Closing exchanges
It is good practice to close exchanges when done, due to potential vunerabilities in a system revealing exchange keys.
```
user1.close_exchange(user2.get_public_key()) # user 1 closes the exchange with user 2
user2.close_exchange(user1.get_public_key()) # user 2 closes the exchange with user 1
```

### Best practices
To make sending and receiving messages as secure as possible, you should follow these simple guidelines:
* Use custom private keys
* Use custom bases
* Use custom modulus'
* Use custom pair values
* You can use a custom epoch scale, but anything other than the default is EXTREMELY unstable

## Other projects

* [hedral](http://www.hedral.info/portfolio/apps) - All my other projects


## Authors

* **James Clarke** - *...Everything*

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* thanks to stack overflow... couldn't have done it without you
