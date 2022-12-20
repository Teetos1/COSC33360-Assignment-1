#include <cmath>
#include <iostream>
#include <pthread.h>
#include <sstream>
#include <vector>

using namespace std;

struct Node {
  string binary;
  double prob;
  double cumux;
  double bar;
  pthread_t threads;
};

//Function threadbar gets an individual symbol and calculates the fbar.
//Calculates the total of 1's and 0's each symbol has.
//Converts the fbar to binary in respect to the length.
void* threadbar(void* ptr) {
  int total = 0;
  Node *p = (Node*)ptr;
  p->bar = (p->cumux - p->prob) + p->prob / 2;
  total = ceil(log2(1 / p->prob)) + 1;

  for(int i = 0; i < total; i++) {
    p->binary += '0';
  }

  int x;
  double j;
  for(x = 0, j = 0.5; x < p->binary.length(); x++, j /= 2) {
    if(j > p->bar)
      continue;
    p->binary[x] = '1';
    p->bar -= j;
  }
}

int main() {
  int count = 0;
  vector<string> sym;
  vector<double> probs;
  string temp;
  double tempf;
  string symbols, freq;
  getline(cin, symbols);
  getline(cin, freq);
  stringstream ss;

  ss << symbols;
  while (ss >> temp) {
    count++;
    sym.push_back(temp);
  }
  ss.clear();

  ss << freq;
  while (ss >> tempf) {
    probs.push_back(tempf);
  }
  ss.clear();

  //Make a temporary variable to store the cumulative x and store it by the corresponding symbol
  //Store the probability of each symbol
  Node p[count];
  tempf = 0;
  for (int i = 0; i < count; i++) {
    tempf += probs[i];
    p[i].cumux = tempf;
    p[i].prob = probs[i];
  }

  //Create threads for each seperate symbol and converts it to binary.
  for(int i = 0; i < count; i++) {
    if(pthread_create(&p[i].threads, NULL, &threadbar, &p[i])) {
      cout << "Error creating thread" << endl;
      return 1;
    }
  }
  for(int i = 0; i < count; i++) {
    pthread_join(p[i].threads, NULL);
  }

  cout << "SHANNON-FANO-ELIAS Codes:" << endl << endl;
  for (int i = 0; i < count; i++) {
    cout << "Symbol " << sym[i] << ", "
         << "Code: " << p[i].binary << endl;
  }

  return 0;
}
