---
layout: project
type: project
image: img/SHA-1(1).jpg
title: "SHA-1"
date: 2022
published: true
labels:
  - C Language
  - Computer Security
  - Hash Algorithm
  
summary: "SHA-1 Final Project for ICS 212."
---

<img class="img-fluid" src="../img/SHA-1(2).png">

The Secure Hash Algorithm 1 (SHA-1) is a cryptographic hash function that creates a hash or message digest from a variable-size input, such as a file or message. It was created in 1993 by the National Institute of Standards and Technology(NIST) of the United States government. 

<hr>

<pre>

This project was completed 







#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
#include <stdbool.h>
#include "printbits.h"

//Aymbolic constants for array sizes
#define MAX_SIZE 1048576
#define WORD_SIZE 80

//Prototype function
unsigned int readFile(unsigned char buffer[]);
unsigned int calculateBlocks(unsigned int sizeOfFileInBytes);
void convertCharArrayToIntArray(unsigned char buffer[], unsigned int message[], unsigned int sizeOfFileInBytes);
void addBitCountToLastBlock(unsigned int message[], unsigned int sizeOfFileInBytes, unsigned int blockCount);
void computeMessageDigest(unsigned int message[], unsigned int blockCount);
unsigned int F(unsigned t, unsigned int B, unsigned int C, unsigned int D);
unsigned int K(unsigned int t);

//Main function
int main(){
    //Declare array  
    unsigned char buffer[MAX_SIZE];
    unsigned int message[MAX_SIZE];
    //Function call
    unsigned int sizeOfFileInBytes = readFile(buffer);
    unsigned int blockCount = calculateBlocks(sizeOfFileInBytes);    
    convertCharArrayToIntArray(buffer,message,sizeOfFileInBytes);
    addBitCountToLastBlock(message,sizeOfFileInBytes,blockCount);
    computeMessageDigest(message, blockCount);
    return 0;

}//End main


//Read file using redirection operator and store each character to buffer array
unsigned int readFile(unsigned char buffer[]){    
    int i = 0;
    char character = '0';
    while((character = fgetc(stdin)) != EOF){   
        //Error check
        if(i > MAX_SIZE){
            printf("ERROR : File size must be less than 1048576\n");
            exit(1);
        }
        buffer[i] = character;    
        //printf("buffer[%i] = %c\n",i,buffer[i]);//Troubleshoot
        i++;
    }
    //Add 1 bit to the end of the array
    buffer[i] = 0x80;    
    return i;
}


//Calculate block needed
unsigned int calculateBlocks(unsigned int sizeOfFileInBytes){
    unsigned int blockCount = (((8 * sizeOfFileInBytes) + 1) / 512) + 1;
    //printf("BlockCount without adding 1 = %i\n",blockCount);//Troubleshoot
    if((((8 * sizeOfFileInBytes) + 1) % 512) > (512 - 64)){
        blockCount = blockCount + 1;    
        //printf("BlockCount after adding 1 = %i\n",blockCount);//Troubleshoot
    }
    return blockCount;
}


//Pack four characters into one integer
void convertCharArrayToIntArray(unsigned char buffer[], unsigned int message[], unsigned int sizeOfFileInBytes){
    int j = 0;
    for(int i = 0; i <= sizeOfFileInBytes; i+=4){
        message[j] = buffer[i] << 24 | buffer[i+1] << 16 | buffer[i+2] << 8 | buffer[i+3];
        //printbits(message[j]);//Troubleshoot
        j++;
    }   
}


//Insert the size of the file in bits into the last index of the last block
void addBitCountToLastBlock(unsigned int message[], unsigned int sizeOfFileInBytes, unsigned int blockCount){
    int sizeOfTheFileInBits = sizeOfFileInBytes * 8;
    int indexOfEndOfLastBlock = (blockCount * 16) - 1;
    message[indexOfEndOfLastBlock] = sizeOfTheFileInBits;
    /*
    for(int i = 0; i <= indexOfEndOfLastBlock; i++){
        printf("%08x\n",message[i]);//Troubleshoot
    }
    */
}


unsigned int f(unsigned int t, unsigned int B, unsigned int C, unsigned int D){    
    if(0 <= t && t <= 19){
        return (B & C) | ((~B) & D);
    }
    else if(20 <= t && t <= 39){
        return (B ^ C ^ D);
    }
    else if(40 <= t && t <= 59){
        return (B & C) | (B & D) | (C & D);
    }
    else if(60 <= t && t <= 79){
        return (B ^ C ^ D);
    }
    return t;
}


//A sequence of constant words K(0), K(1), ... , K(79)
unsigned int k(unsigned int t){
    if(0 <= t && t <= 19){
        return 0x5A827999;
    }
    else if(20 <= t && t <= 39){
        return 0x6ED9EBA1;
    }
    else if(40 <= t && t <= 59){
        return 0x8F1BBCDC;
    }
    else if(60 <= t && t <= 79){
        return 0xCA62C1D6;
    }
    return t;
}


void computeMessageDigest(unsigned int message[], unsigned int blockCount){       
    //Initialized variables in hex
    unsigned int H0 = 0x67452301;
    unsigned int H1 = 0xEFCDAB89;
    unsigned int H2 = 0x98BADCFE;
    unsigned int H3 = 0x10325476;
    unsigned int H4 = 0xC3D2E1F0;

    unsigned W[WORD_SIZE];
    for(int block = 0; block<blockCount; block++){    
        /*
        if(blockCount <= 2){
            printf("H0 = %08x\nH1 = %08x\nH2 = %08x\nH3 = %08x\nH4 = %08x\n", H0, H1, H2, H3, H4);//Troubleshoot
        }
        */
        //Divide message into 16 words
        for (int t = 0; t <= 79; t++){
            if(t < 16){
                W[t] =  message[t+(16*block)];
                if(blockCount <= 2){
                    //Words for block
                    printf("W[%i] = %08x\n",t,W[t]);
                }   
            }
            else{
                W[t] = ((W[t-3] ^ W[t-8] ^ W[t-14] ^ W[t-16]) << 1) | ((W[t-3] ^ W[t-8] ^ W[t-14] ^ W[t-16]) >> (32 - 1));
                //printf("W[%i] = %08x\n",t,W[t]);//Troubleshoot
            }
        }

        unsigned int A = H0;
        unsigned int B = H1;
        unsigned int C = H2;
        unsigned int D = H3;
        unsigned int E = H4;

        //Computation 
        for(int t = 0; t <= 79; t++){
            unsigned int F = 0;
            unsigned int K = 0;
            if(0 <= t && t <= 19){      
                F = f(t, B, C, D);
                K = k(t);
            }
            else if(20 <= t && t <= 39){
                F = f(t, B, C, D);
                K = k(t);
            }
            else if(40 <= t && t <= 59){
                F = f(t, B, C, D);
                K = k(t);
            }
            else if(60 <= t && t <= 79){
                F = f(t, B, C, D);
                K = k(t);
            }            
            
            //Left shift A five bits than add F, E, W[t] and K 
            unsigned int temp = ((A << 5) | (A >> (32 - 5))) + F + E + W[t] + K;
            E = D;
            D = C;
            C = (B << 30) | (B >> (32 - 30));
            B = A;
            A = temp;
                   
            if(blockCount <= 2){
                printf("t = %i: %08x %08x %08x %08x %08x\n", t, A, B, C, D, E);
            }
        }

        //Add the result to each variable
        H0 = H0 + A;
        H1 = H1 + B;
        H2 = H2 + C;
        H3 = H3 + D;
        H4 = H4 + E;
    }
    //Message digest
    printf("Message digest = %08x %08x %08x %08x %08x\n", H0, H1, H2, H3, H4);
}

</pre>

<hr>

Source: <a href="https://github.com/hokwaichan/ICS212FinalProject"><i class="large github icon "></i>finalProject/ics-212-SHA-1</a>
