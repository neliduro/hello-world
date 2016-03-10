# hello-world
Hello!!!


"""
A.Match_ends
"""

def match_ends(words):
    i=0
    sortie=0
    while i <len(words):
        ti=len(words[i])
        if len(words[i])>=2 and words[i][0] ==words[i][ti-1]:
            sortie=sortie+1
        
    
        i=i+1
    #print sortie
    return sortie
    
    
words=["tata", "toto", "a", "ata", "pap"]
match_ends(words)


words=["tata", "toto", "a", "ata", "pap", "papappaap"]
match_ends(words)
    
"""
B.Front_x
"""

def front_x(words):
    i=0
    sortie=[]
    fin=[]
    ex=[]
    while i <len(words):
        if words[i][0]=="x":
            ex.append(words[i])
        else:
            fin.append(words[i])
        i=i+1
    i=0
    while i <len(ex):
        t=i+1  
        sortie.append(ex[i])
        while t<len(ex):
            if sortie[i]>ex[t]:
                temp=sortie[i]
                sortie[i]=ex[t]
                ex[t]=temp
            t=t+1
        i=i+1
    i=0        
    while i <len(fin):
        t=i+1   
        sortie.append(fin[i])
        while t<len(fin):
            if sortie[len(ex)+i]>fin[t]:
                temp=sortie[len(ex)+i]
                sortie[len(ex)+i]=fin[t]
                fin[t]=temp
            t=t+1

        i=i+1
    #print sortie
    return sortie
words=["tata", "toto", "a", "ata", "xu","pap", "xoo", "xaaa", "xeeeee"]
front_x(words)


"""
C sort_last
"""
def sort_last(tuples):
    i=0
    sortie=[]
    while i<len(tuples):
        t=i+1
        sortie.append(tuples[i])
        while t<len(tuples):
            if sortie[i][-1]>tuples[t][-1]:
                temp=sortie[i]
                sortie[i]=tuples[t]
                tuples[t]=temp
            t=t+1

        i=i+1
    #print sortie
    return sortie
    
tuples=[(1, 7), (1, 3), (3, 4, 5), (2, 2)]
sort_last(tuples)

def test(got, expected):
    if got == expected:
        prefix = ' OK '
    else:
        prefix = '  X '
    print '%s got: %s expected: %s' % (prefix, repr(got), repr(expected))
    
def main():
    print 'match_ends'
    test(match_ends(['aba', 'xyz', 'aa', 'x', 'bbb']), 3)
    test(match_ends(['', 'x', 'xy', 'xyx', 'xx']), 2)
    test(match_ends(['aaa', 'be', 'abc', 'hello']), 1)

    print
    print 'front_x'
    test(front_x(['bbb', 'ccc', 'axx', 'xzz', 'xaa']),
        ['xaa', 'xzz', 'axx', 'bbb', 'ccc'])
    test(front_x(['ccc', 'bbb', 'aaa', 'xcc', 'xaa']),
        ['xaa', 'xcc', 'aaa', 'bbb', 'ccc'])
    test(front_x(['mix', 'xyz', 'apple', 'xanadu', 'aardvark']),
        ['xanadu', 'xyz', 'aardvark', 'apple', 'mix'])
        
main()


"""
D. remove_adjacent
"""
def remove_adjacent(nums):
    sortie=[]
    i=1
    if len(nums)>1:
        sortie.append(nums[0])
        while i<len(nums):
            if nums[i-1] !=nums[i]:
                sortie.append(nums[i])
            i=i+1
        print sortie
    else:
        sortie=nums
        #print nums
    return sortie
    
nums= [1, 2, 2, 3]
remove_adjacent(nums)


"""""""""""""""
E. linear_merge
"""""""""""""""
def linear_merge(list1, list2):
    out=list1+list2
    sortie=[]
    i=0
    while i <len(out):
        t=i+1  
        sortie.append(out[i])
        while t<len(out):
            if sortie[i]>out[t]:
                temp=sortie[i]
                sortie[i]=out[t]
                out[t]=temp
            t=t+1
        i=i+1
    #print sortie
    return sortie
    
list1= [1, 2, 3]
list2=[2,3,5]
linear_merge(list1, list2)




def test(got, expected):
    if got == expected:
        prefix = ' OK '
    else:
        prefix = '  X '
    print '%s got: %s expected: %s' % (prefix, repr(got), repr(expected))
    return
    
def main():
    print 'remove_adjacent'
    test(remove_adjacent([1, 2, 2, 3]), [1, 2, 3])
    test(remove_adjacent([2, 2, 3, 3, 3]), [2, 3])
    test(remove_adjacent([]), [])

    print
    print 'linear_merge'
    test(linear_merge(['aa', 'xx', 'zz'], ['bb', 'cc']),
        ['aa', 'bb', 'cc', 'xx', 'zz'])
    test(linear_merge(['aa', 'xx'], ['bb', 'cc', 'zz']),
        ['aa', 'bb', 'cc', 'xx', 'zz'])
    test(linear_merge(['aa', 'aa'], ['aa', 'bb', 'bb']),
        ['aa', 'aa', 'aa', 'bb', 'bb'])
    return
        
main()
    
    

    
"""
A. donuts
"""

def donuts(count):
    if count<10: 
        return 'Number of donuts: %s' % (repr(count))
    else:
        return 'Number of donuts: many'
    
    
donuts(5)
donuts(11)


"""
B. both_ends
"""

def both_ends(s):
    if len(s)<=2:
        return ''
    else: 
        return s[0]+s[1]+s[-2]+s[-1]       
    

both_ends('valable')
both_ends('ok')



"""
C. fix_start
"""
def fix_start(s):
    temp=s[0]
    tempo=s[1:len(s)]
    sort=tempo.replace(temp, '*')
    sortie=s[0]+sort
    #print sortie
    return sortie

fix_start('babble')



"""
D. MixUp
"""

def mix_up(a, b):
    atemp=a.replace(a[0], b[0])
    atemp=atemp.replace(a[1], b[1])
    btemp=b.replace(b[0], a[0])
    btemp=btemp.replace(b[1], a[1])
    sortie=atemp+' '+btemp
    #print sortie    
    
    return sortie
    
mix_up('dog', 'dinner')
mix_up('mix', 'pod')



def test(got, expected):
    if got == expected:
        prefix = ' OK '
    else:
        prefix = '  X '
    print '%s got: %s expected: %s' % (prefix, repr(got), repr(expected))
    return


def main():
    print 'donuts'
    # Each line calls donuts, compares its result to the expected for that call.
    test(donuts(4), 'Number of donuts: 4')
    test(donuts(9), 'Number of donuts: 9')
    test(donuts(10), 'Number of donuts: many')
    test(donuts(99), 'Number of donuts: many')

    print
    print 'both_ends'
    test(both_ends('spring'), 'spng')
    test(both_ends('Hello'), 'Helo')
    test(both_ends('a'), '')
    test(both_ends('xyz'), 'xyyz')

  
    print
    print 'fix_start'
    test(fix_start('babble'), 'ba**le')
    test(fix_start('aardvark'), 'a*rdv*rk')
    test(fix_start('google'), 'goo*le')
    test(fix_start('donut'), 'donut')

    print
    print 'mix_up'
    test(mix_up('mix', 'pod'), 'pox mid')
    test(mix_up('dog', 'dinner'), 'dig donner')
    test(mix_up('gnash', 'sport'), 'spash gnort')
    test(mix_up('pezzy', 'firm'), 'fizzy perm')
    return


main()



"""
D. verbing
"""

def verbing(s):
    if len(s)<=3 :
        return s 
        
    else:
        temp=s[-3:len(s)]
        if temp=='ing':
            sortie=s+'ly'
            return sortie
        else:
            sortie=s+'ing'
            return sortie
    
    
verbing('tapating')


"""
E. not_bad
"""

def not_bad(s):
    i=0
    t=0
    debut=0
    fin=0        
    while i<len(s):
        temp=s[i:i+3]
        if temp=='not':
            debut=i
            t=t+1
        if temp=='bad':
            fin=i+3
            t=t+1
        i=i+1
        if t==2:
            i=len(s)        
    if debut<fin:
        sort=s.replace(s[debut:fin], 'good')
        return sort
    else: 
        return s
    
    
s='This dinner is not that bad!'
not_bad(s)
s='This dinner is bad and not ok!'
not_bad(s)



"""
F. front_back
"""

def front_back(a, b):
    l_a=len(a)
    l_b=len(b)
    if l_a%2==0 :
        a_front=a[0:l_a/2]
        a_back=a[l_a/2:l_a]
    else:
        a_front=a[0:l_a/2+1]
        a_back=a[l_a/2+1:l_a]
    if l_b%2==0 :
        b_front=b[0:l_b/2]
        b_back=b[l_b/2:l_b]
    else:
        b_front=b[0:l_b/2+1]
        b_back=b[l_b/2+1:l_b]
    return a_front+b_front+a_back+b_back
    
front_back('abcde', 'efgh')


    
def main():
    print 'verbing'
    test(verbing('hail'), 'hailing')
    test(verbing('swiming'), 'swimingly')
    test(verbing('do'), 'do')
    
    
    print 'not_bad'
    test(not_bad('This movie is not so bad'), 'This movie is good')
    test(not_bad('This dinner is not that bad!'), 'This dinner is good!')
    test(not_bad('This tea is not hot'), 'This tea is not hot')
    test(not_bad("It's bad yet not"), "It's bad yet not")

    
    print 'front_back'
    test(front_back('abcd', 'xy'), 'abxcdy')
    test(front_back('abcde', 'xyz'), 'abcxydez')
    test(front_back('Kitten', 'Donut'), 'KitDontenut')
    return
main()


    
    
    
    
    





