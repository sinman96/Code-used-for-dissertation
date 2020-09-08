# Code-used-for-dissertation
All the code used for obtaining the results of my Master's thesis

def intersection(lst1, lst2):
"""
Inbuilt python function that finds the common elements between two lists, is lists are disjoint it
will return an empty list, put into main file for visualisation and to avoid imports.
"""
    lst3 = [value for value in lst1 if value in lst2] 
    return lst3

def regfuz(r1,r2):
    """Returns the common elements of two lists being compared, and the proportion
       of common elements r1 and r2 have in terms of  the list in r1's length, if the lists are not the 
       same length the empty set is returned. It also works out the average amount of strings accepted by r2 that are als0 accepted by r1 as a proportion
       between 0 and 1 and this p value for each sublist being compared."""
    if (len(r1)==len(r2)):
       k = []
       l = []
       m = []
       n = len(r1)
       for i in range(0,n):
           k.append([r1[i],r2[i]])
           l.append(intersection(k[i][0],k[i][1]))
           m.append(len(l[i])/len(k[i][0]))
       p = sum(m)/len(m)
       return [l,m,p]
    if (len(r1)!=len(r2)):
         return [[]]
     
def listprod(x):
    """Is the product of all the lengths of the sublists within a list"""
    p =1
    for i in range(0,len(x)):
        p = p*len(x[i])
    return p
     
def diff(l):
    """Simplifies lists of numbers and Latin alphabet characters in ranges when they are, else just returns them as their original lists """
    if len(l) == 1:
        return l
    a = l[0]
    b = l[-1] 
    c = list(map(chr, range(97, 123)))
    d = list(map(chr, range(65, 91)))
    e = []
    e1 = []
    if isinstance(a,int)==True and isinstance(b,int)==True:
        b = b+1
        if (l == list(range(a,b,1))):        
            return [a,'-',b-1]
        else:        
            return [l]
    if (a in c) ==True and (b in c) ==True:
        for i in range(0,len(c)):
            if c[i]==a:
                e.append(i)
            if c[i] == b:
               e1.append(i)
        y1 = e[0]+97
        y2 = e1[0]+98
        z =  list(map(chr, range(y1, y2)))            
        if (l == z):        
              return [z[0],'-',z[-1]]
        else:        
            return [l]
    if (a in d) ==True and (b in d) ==True:
        for i in range(0,len(d)):
            if d[i]==a:
                e.append(i)
            if d[i] == b:
               e1.append(i)
        y1 = e[0]+65
        y2 = e1[0]+66
        z =  list(map(chr, range(y1, y2)))            
        if (l == z):        
              return [z[0],'-',z[-1]]
        else:        
            return [l]
def regcom(r1,r2,disjunct):
    """Calculates triples of the number strings accepted by r1 and not r2,the number of strings accepted by both r1 and r2 and the number strings accepted by r1 and not r2
    disjunct = 0 means the regexp does not have an or clause, disjunct = 1 means the regexp has an or clause."""
    if (disjunct == 0):
       if(len(r1) == len(r2)):    
           p = 1
           p1 =1
           p2 =1     
           a = regfuz(r1,r2)[0]     
           for i in range(0,len(a)):
               p = p*len(a[i])
           for i in range(0,len(r1)):
               p1 = p1*len(r1[i])
           for i in range(0,len(r2)):
               p2 = p2*len(r2[i])
           if p1-p == 0 or p2-p ==0:
              return[p1-p,p,p2-p,regfuz(r1,r2)[0],"A complete match between R1 and R2"]
           if p ==0:
              return[p1-p,p,p2-p,regfuz(r1,r2)[0],"No match between R1 and R2"]
           else:
                return [p1-p,p,p2-p,regfuz(r1,r2)[0],"Proportion of strings accepted by r1 also accepted by r2 equals",p/p1,"Proportion of strings accepted by r2 also accepted by r1 equals",p/p2]
       if (len(r1) != len(r2)):
           p = 0
           p1 =1
           p2 =1       
           for i in range(0,len(r1)):
               p1 = p1*len(r1[i])
           for i in range(0,len(r2)):
               p2 = p2*len(r2[i])                            
           return [p1,p,p2,'No regular expression accepted by R1 and R2']
    if (disjunct == 1):
            p = 0
            p1 =0 
            p2 = 0
            m = len(r1)
            n = len(r2)
            count=0
            out=[0]*(m*n)        
            for i in range(n):
                for j in range(m):
                    out[count] =regfuz(r1[j] ,r2[i])[0]
                    count+=1         
            while [] in out:
                  out.remove([])
            for i in range(0,len(out)):
                p = p+ listprod(out[i])
            for i in range(0,m):
                p1 = p1+listprod(r1[i])
            for i in range(0,n):
                p2 = p2+listprod(r2[i])
            if p1-p==0 or p2-p == 0:
               return [p1-p,p,p2-p,out,"A complete match between R1 and R2"]
            if p==0:
               return [p1-p,p,p2-p,out,"No match between R1 and R2"]
            else:
                return [p1-p,p,p2-p,out,"Proportion of strings accepted by r1 also accepted by r2 equals",p/p1,"Proportion of strings accepted by r2 also accepted by r1 equals",p/p2]     



def all_exp(a):
    """Gives all possible regular expressions of a regular expression 'a' inputted as a list,expanded in full form
    disjunct = 0 means the regexp does not have an or clause, disjunct = 1 means the regexp has an or clause."""
    for i in range(0,len(a)):
        while [] in a[i]:
            a.remove(a[i])
    return a


def common_exp(x,disjunct):
    """Gives all possible regular expressions of a regular expression 'x' inputted as a list,expanded in simplified form.
    It also abbreviates them where possible. disjunct = 0 means the regexp does not have an or clause, disjunct = 1 means the regexp has an or clause."""
    h =[]
    if disjunct == 0:
       for i in range(0,len(x)):
           h.append(diff(x[i]))
       return 'Regexps accepted by R1 and R2 are:',h
    if disjunct ==1:
        y = []
        z = []
        for i in range(0,len(x)):
            for j in range(0,len(x[i])):
                y.append(diff(x[i][j]))
        for k in range(0,len(x)):
            z.append(len(x[i]))
        z1 = [0]*(len(z))
        for i in range(1,len(z)):
            z1[0] = z[0]
            z1[i] = z[i]+z1[i-1]
        a = [[y[0:z1[0]]]]
        for i in range(0,len(z)-1):
            a.append([y[z1[i]:z1[i+1]]])
        return 'Regexps accepted by R1 and R2 are:',a
