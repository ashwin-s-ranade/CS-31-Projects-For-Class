#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <cctype>
#include <cstring>
#include <cassert>

using namespace std;
const int MAX_WORD_LENGTH = 20;

//function prototypes
int makeProper(char [][MAX_WORD_LENGTH + 1], char [][MAX_WORD_LENGTH + 1], int[], int); 
void removeRow(int, char[][MAX_WORD_LENGTH + 1], char[][MAX_WORD_LENGTH +1], int[], int);
int rate(const char document[], const char word1[][MAX_WORD_LENGTH + 1], 
	const char word2[][MAX_WORD_LENGTH + 1], const int separation[], int nPatterns);
int closestIndex(const char [], char [][MAX_WORD_LENGTH + 1], int , int );


int main() {

}

void removeRow(int row, char word1[][MAX_WORD_LENGTH + 1], char word2[][MAX_WORD_LENGTH + 1], int separation[], int nPatterns) {
	//shift everything in array over one 
	for (int i = row; i < nPatterns - 1; i++) {
		strcpy(word1[i], word1[i + 1]);
		strcpy(word2[i], word2[i + 1]);
		separation[i] = separation[i + 1];
	}
}

int makeProper(char word1[][MAX_WORD_LENGTH + 1], char word2[][MAX_WORD_LENGTH + 1], int separation[], int nPatterns) {

	if (nPatterns <= 0) { return 0; }//if there are no patterns, then no patterns are in proper form 

	//if a word in the pattern contains no characters, or a character that is not a letter, remove from collection
	for (int i = 0; i < nPatterns; i++) {
		for (int j = 0; j < strlen(word1[i]); j++) { //check word 1 array

			if (!isalpha(word1[i][j])) { //if not a letter, remove pattern from collection
				removeRow(i, word1, word2, separation, nPatterns);
				nPatterns--;
				i--; //checks same index, but this time will be different array 
			}

		}

		for (int j = 0; j < strlen(word2[i]); j++) { //check word 2 array
			if (!isalpha(word2[i][j])) { //if not a letter, remove pattern from collection
				removeRow(i, word1, word2, separation, nPatterns);
				nPatterns--;
				i--;
			}

		}

		if (separation[i] < 0) { //check separation array 
			removeRow(i, word1, word2, separation, nPatterns);
			nPatterns--;
			i--;
		}

	}

	//now, make all the remaining characters lowercase
	for (int i = 0; i < nPatterns; i++) {
		for (int j = 0; j < strlen(word1[i]); j++) {
			if (!islower(word1[i][j]))
				word1[i][j] = tolower(word1[i][j]);
		}
	}
	for (int i = 0; i < nPatterns; i++) {
		for (int j = 0; j < strlen(word2[i]); j++) {
			if (!islower(word2[i][j]))
				word2[i][j] = tolower(word2[i][j]);
		}
	}

	//finally, remove if pattern have the same w1 and w2 combination
	for (int i = 0; i < nPatterns; i++) {
		for (int j = i + 1; j < nPatterns; j++) {
			//if the two patterns have the same word 1 and word 2 values, remove the pattern with the lower seperation (keep the higher seperation)
			if ((strcmp(word1[i], word1[j]) == 0 && strcmp(word2[i], word2[j]) == 0 && i != j) ||
				(strcmp(word1[i], word2[j]) == 0 && strcmp(word2[i], word1[j]) == 0 && i != j)) {
				if (separation[i] >= separation[j])
					removeRow(j, word1, word2, separation, nPatterns);
				else
					removeRow(i, word1, word2, separation, nPatterns);
				nPatterns--;
				i--;
				j--;
			}
		}
	}


	return nPatterns;
}

int rate(const char document[], const char word1[][MAX_WORD_LENGTH + 1], const char word2[][MAX_WORD_LENGTH + 1],
	const int separation[], int nPatterns) {
	//we are allowed to assume the last 4 parameters, which represent a collection of patterns, are in proper form
	//const char document[] is guaranteed to be no longer than 250 characters 
	//a negative nPatterns param is treated as 0; returns rating (# of patterns document matches)

	//edge case for empty string document
	if (strlen(document) == 0)
		return 0; 


	if (nPatterns <= 0) { return 0; }//if there are no patterns, then no patterns match w/ the document 
	char docCopy[251];
	strcpy(docCopy, document); 
	
	//remove all non-alphabetic characters other than spaces and make all upper case letters lower case 
	for (int i = 0; i < strlen(docCopy); i++) {
		if (isupper(docCopy[i]))
			docCopy[i] = tolower(docCopy[i]);

		if (!isalpha(docCopy[i]) && docCopy[i] != ' ') {
			for (int j = i; j < strlen(docCopy); j++)
				docCopy[j] = docCopy[j + 1];
			i--;
		}
	}
	
	//make all the spaces in document only 1 space long 
	for (int i = 0; i < strlen(docCopy)-1; i++){
		if (docCopy[i] == ' ' && docCopy[i + 1] == ' ') {
			for (int j = i; j < strlen(docCopy); j++)
				docCopy[j] = docCopy[j + 1];
			i--;
		}
	}
	//make sure the docCopy doesn't start with a space
	for (int i = 0; i < strlen(docCopy) - 1; i++) {
		if (docCopy[0] == ' ') {
			for (int j = i; j < strlen(docCopy); j++)
				docCopy[j] = docCopy[j + 1];
			i--;
		}

	}
	 
	//create an array of cstrings based on every string 
	int numberOfWords = 0; //use this variable to make sure you only look at valid words in 'words' array 
	char words[250][MAX_WORD_LENGTH + 1];
	

	int count = 0;
	for (int i = 0; i < strlen(docCopy); i++) {
		if (docCopy[i] == ' ') {
			words[numberOfWords][count] = '\0'; //add 0 byte to make it a cstring 
			numberOfWords++;
			count = 0;
		}
		else {
			words[numberOfWords][count] = docCopy[i];
			count++;
		}
	}
	words[numberOfWords][count] = '\0'; //add 0 byte on the last word 
	numberOfWords++;

	/*for (int i = 0; i < numberOfWords; i++)
	{
		for (int j = 0; j < strlen(words[i]); j++)
			cerr << words[i][j];
		cerr << endl;
	}*/

	int rating = 0; 
	//check if separation between word1 and word2 is satisfactory


	for (int i = 0; i < nPatterns; i++) {
		for (int j = 0; j < numberOfWords; j++) {
			if (strcmp(word1[i], words[j]) == 0) {
				//return index that matches word2 after word1

				if ((closestIndex(word2[i], words, j, numberOfWords) - j) <= separation[i]+1) {
					rating++; 
					break; //now we should reset i to the next index, this break wil get us out of the loop
				}	
			}
			if (strcmp(word2[i], words[j]) == 0) {
				//return index that matches word2 after word1

				if ((closestIndex(word1[i], words, j, numberOfWords) - j) <= separation[i] + 1) {
					rating++;
					break; //now we should reset i to the next index, this break wil get us out of the loop
				}
			}
		}
	}



	return rating; 

}

int closestIndex(const char theWordInQuestion[], char wordsArray[][MAX_WORD_LENGTH + 1], int index, int patterns) {
	if (index + 1 >= patterns)
		return 260; 
	for (int i = index+1; i < patterns; i++)
	{
		if (strcmp(theWordInQuestion, wordsArray[i]) == 0)
			return i; 
	}
	return 260; //just a number longer than the amt of words that can possibly be in the document 
}
