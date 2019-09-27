# MachineLearning
import sys
import math
import random as rnd
from collections import defaultdict
import csv

def distance(data, k):
    sm = sum([(m - n)**2 for m, n in zip(data, k)])
    return math.sqrt(sm)

def find_cluster(data, k):
    d = defaultdict(list)
    cen = []
    for i in range(0,len(data)):
        temp = []
        for j in range(0,len(k)):
            temp.append(distance(data[i], k[j]))
        idx = temp.index(min(temp))
        d[idx].append(data[i])
    for key in d:
        temp = d[key]
        cen.append([sum(l)/len(l) for l in zip(*temp)])
    return cen

def has_converged(old, new):
    return (set([tuple(a) for a in old]) == set([tuple(a) for a in new]))

def k_means(data, k):
    c1 = rnd.sample(data, k)
    while True:
        c1_new = find_cluster(data, c1)
        if has_converged(c1, c1_new):
            c1  = c1_new
            break
        c1  = c1_new
    for i in range(0,len(data)):
        temp = []
        for j in range(0,len(c1)):
            temp.append(distance(data[i], c1[j]))
        idx = temp.index(min(temp))
        print(idx, i)
    return c1

if __name__ == '__main__':
    
    datafile = sys.argv[1]
    f=open(datafile)    
    data=[]
    i=0;
    l=f.readline()
    while (l != ''):
        a=l.split()
        l2=[]
        for j in range(0, len(a), 1):
            l2.append(float(a[j]))
        data.append(l2) 
        l=f.readline()
    k=sys.argv[2]

    mu = k_means(data, int(k))
