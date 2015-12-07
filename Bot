
##########################
## Vibhanshu Bhardwaj
## University of Waterloo
## reddit bot that encrypts comments when called
## Makes use of the RSA encryption technique.
##########################

import praw
import pdb
import re
import time
import os
from pprint import pprint

#     RSA SETUP   

p = 77269   # p and q are the primes of RSA.
            # Currently, these values are being used by the bot.
q = 95539   # Feel free to change the value to other primes (preferably large)


public_key = 2741253329

private_key = 3480660329

n = p*q       # n of RSA.

# phi_function = (public_key - 1) * (private_key - 1) for the sake of completion.


## exp_modulo(message,key_exponent) computes message**key_exponent(mod n)
##      very quickly using properties of congruence.
## exp_modulo: Nat, Nat -> Nat

def exp_modulo(message,key_exponent):
    if key_exponent == 1:
        return message % n
    if (key_exponent % 2) == 1:
        return (exp_modulo(((message ** 2) % n), (key_exponent / 2)) * message) % n
    else:
        return exp_modulo(((message ** 2) % n), (key_exponent / 2))


## encrypt_message(message) encrypts message to cipher according to RSA.
## encrypt_message: Nat -> Nat

def encrypt_message(message):
    return exp_modulo(message, public_key)


## decrypt_cipher(cipher) decrypts cipher to the original message.
## decrypt_cipher: Nat -> Nat

def decrypt_cipher (cipher):
    return exp_modulo(cipher, private_key)


## encrypt(message) encrypts message by first converting message (string) to
##      a list of characters, then converting each character to its ASCII
##      representation, and then encrypting each number using encrypt_message.
## encrypt: Str -> (listof Int)

def encrypt(message):
    lochar = list(message) # converting to list of characters
    encrypted_lochar = []  # initial value of the encrypted list.
    for char in lochar:
        encrypted_lochar.append(encrypt_message((ord(char)))) #performing encryption
    return encrypted_lochar


## decrypt(loint) decrypts loint (a list of integers) by decrypting each integer
##      integer in the list, then converting each integer back to its char value
##      and then concatenates all the chars to form the original encrypted string.
## decrypt: (listof Int) -> Str

def decrypt(loint):
    decrypted_lochar=[]  # initial value of the decrypted list of characters.
    for integer in loint:
        decrypted_lochar.append(chr(decrypt_cipher(integer))) #performing decryption
    return ''.join(decrypted_lochar) #converting the list to string.


##   Code for Bot ##

r = praw.Reddit ('RSA encryptor and decryptor by Vibhanshu3')
r.login('USERNAME', 'PASSWORD', disable_warning=True)

# we use a file that will store list of comments it has already encrypted
if not os.path.isfile("comments_encrypted.txt"):  
    comments_encrypted = []                 #we create a list of ids of the comments that have been encrypted
    
## if the program has run before, it loads the list of ids of comments that have been encrypted 
##    so as to not encrypt the same comment twice.
else:
    with open("comments_encrypted.txt", "r") as f:
        comments_encrypted = f.read()   
        comments_encrypted = comments_encrypted.split("\n")
        comments_encrypted = filter(None, comments_encrypted)



        
keyword = "Encrypt: "  # the 'command' that 'calls' the bot. This is what the comment should start with for the bot to encrypt it.

# a while loop to keep it running forever
while True:
    
    subreddit = r.get_subreddit ('test')  #the subreddit we're searching.
    all_comments = subreddit.get_comments()
    flat_comments = praw.helpers.flatten_tree(all_comments)   #to make search easier
    
    for comment in flat_comments:
        if keyword == comment.body[0:9] and comment.id not in comments_encrypted:
            #this is where actual encyption takes place.
            comment.reply("Encrypted comment: " + str(encrypt(comment.body[9:])) + "\n" + "\n" + "This action was performed by a bot. Please PM me in case of queries/concerns.")
            comments_encrypted.append(comment.id)
            with open("comments_encrypted.txt", "a") as f:
                f.write(comment.id + "\n")      # we keep adding new comment ids to the file
                
    time.sleep(60)








