# Problem Set #5

## Problem #1: 3.13
Skip this.

## Problem #2
Assume	that	your	architecture	has	a	test-and-set	instruction	as	its	only	atomic	
primitive.		Implement	atomic	compare-and-exchange	out	of	the	test-and-set	primitive.

Solution:
```
typedef struct cae_t
{
   int value;
   int lock;  // 1 denotes locked
} cae_t;
// prototype of test_and_set
// old value returned
int test_and_set(int * mem);
// returns 1 if compare success
// returns 0 if compare failure
int compare_and_exchange(cae_t * mem, int compare_value, int swap_value)
{
   int ret_value = 0;
   // grab lock
   while(test_and_set(&(mem‐>lock)){}
   if(compare_value == mem‐>value)
   {
      mem‐>value = swap_value;
      ret_value = 1;
   }
   else
   {
      ret_value = 0;
   }
   // release lock
   mem‐>lock = 0;
} 
```


## Problem #3
List	the	possible	sequentially	consistent	outcomes	for	the	variables	i	and	j	after	
the	completion	of	executing	the	three	threads	T1,	T2,	and	T3.		Assume	that	all	threads	begin	executing	
after	‘i’	has	been	set	to	9	and	‘j’	is	set	to	10.
i = 9, j = 10
```
T1:
ADDI	R1,	R0,	30
SW	R1,	0(i)
LW	R2,	0(j)
SW	R2,	0(j)

T2:
ADDI	R5,	R0,	99
LW	R6,	0(j)
ADD	R7,	R5,	R6
SW	R7,	0(j)

T3:
ADD	R8,	R0,	100
SW	R8,	0(i)
```

Solution:
```
We first investigate the final value for ‘i’.  Because thread T3 only has one memory operation, that
memory operation is either ordered before or after the store to ‘i’ in thread T1, therefore `i` is either 30
or 100 and these outcomes are valid with all outcomes of ‘j’.
Now we investigate valid values for ‘j’.
Case 1:
T2 LW R6, 0(j) j = 10
T2 SW R7, 0(j) j = 109
T1 LW R2, 0(j) j = 109
T1 SW R2, 0(j) j = 109
Case 2:
T2 LW R6, 0(j) j = 10
T1 LW R2, 0(j) j = 10
T2 SW R7, 0(j) j = 109
T1 SW R2, 0(j) j = 10
Case 2:
T2 LW R6, 0(j) j = 10
T1 LW R2, 0(j) j = 10
T1 SW R2, 0(j) j = 10
T2 SW R7, 0(j) j = 109
Case 3:
T1 LW R2, 0(j) j = 10
T2 LW R6, 0(j) j = 10
T2 SW R7, 0(j) j = 109
T1 SW R2, 0(j) j = 10
Case 4:
T1 LW R2, 0(j) j = 10
T2 LW R6, 0(j) j = 10
T1 SW R2, 0(j) j = 10
T2 SW R7, 0(j) j = 109
   
Case 5:
T1 LW R2, 0(j) j = 10
T1 SW R2, 0(j) j = 10
T2 LW R6, 0(j) j = 10
T2 SW R7, 0(j) j = 109

Therefore, valid sequentially consistent outcomes are {i, j} = {30, 10}, {30,
109}, {100, 10}, and {100, 109}
```			

## Problem #4
You	are	writing	a	multi-threaded	program	that	will	count	the	number	of	
occurrences of	a	value	in	an	array.		The	values	in	the	array	are	between	0	and	1023.		In	effect,	you	will	
be	building	a	histogram.		Assume	that	the	list	of	numbers	is	very	large,	on	the	order	of	gigabytes	large.		
Extend	the	following	program	such	that	100	threads	(processors)	can	execute	on	the	program	
concurrently.		Assume	a	sequentially	consistent	memory	model.		Add	P()	and	V()	semaphores	where	
appropriate	and	add	any	storage	needed	for	the	semaphores.		Explain	why	the	speedup	of	such	a	
solution	may	not	be	100x. Note	that	the	output	lock	array	is	assumed	to	be	initialized	to	1	(this	allows	
for	a	mutex).
```
//	Sequential	code,	assume	that	the	input	and	output	arrays	are	created
//	outside	of	the	function
#define	MAX_VALUE	1023
function(int	input_array_size,	int	*	input_array,	int	*	output_array)
{
			int	counter;
			for(counter	=	0;	counter	<	input_array_size;	counter++)
			{
						assert(input_array[counter]	<=	MAX_VALUE);
						assert(input_array[counter]	>=	0);
						output_array[input_array[counter]]++;
			}
}
```
Answer: I don't know

## Problem #5
Show	for	each	cache	line	and	cache	what	state	it	is	in	on	every	cycle	assuming	
three	processors	executing	code	as	interleaved	below.		Assume	a	64-byte	cache	line	block	size.		Assume	
all	cores	contain	a	direct	mapped	cache	that	is	4KB	large.		First,	assume	that	the	processors	are	using	a	
snoopy	MSI	cache	coherence	protocol.		Second,	repeat	this	for	a	MESI	protocol.

```
Time
	P1:			  P2: 				P3:
1  	LW R1,	4(R0)			
2				  LW	R1,	16(R0)
3								LW	R1,	4(R0)
4								SW	R2,	100(R0)
5								LW	R4,	104(R0)
6				  LW	R3,	100(R0)
7  	SW	R1,	0(R0)
8	LW	R1,	4100(R0)
9	SW	R2,	4100(R0)
10				  SW	R3,	4100(R0)
11								SW	R5,	0(R0)
```

I'm assuming all caches are reset in the beginning, so all lines are invalid.

MSI
```
Time
	C1:			  C2: 				C3:
1  	Line 0: S		
2	Line 0: S                 Line 0: S			  
3	Line 0: S                 Line 0: S                     Line 0: S
4	Line 0: S		  Line 0: S			Line 0: S
							        Line 1: M
5	Line 0: S		  Line 0: S			Line 0: S
							        Line 1: M							
6	Line 0: S                 Line 0: S                     Line 0: S
                                  Line 1: S                     Line 1: S
7       Line 0: M                 Line 1: S                     Line 1: S
8       Line 64: S                Line 1: S                     Line 1: S
9       Line 64: M                Line 1: S                     Line 1: S
10                                Line 1: S                     Line 1: S
				  Line 64: M
11				  Line 1: S                     Line 1: S
                                  Line 64: M                    Line 0: M
```

MESI
```
Time
	C1:			  C2: 				C3:
1  	0: E
2       0: S                      0: S
3       0: S                      0: S                          0: S
4       0: S                      0: S                          0: S
                                                                64: M
5       0: S                      0: S                          0: S
                                                                64: M
6       0: S                      0: S                          0: S
                                  64: S                         64: S
7       0: M                      64: S                         64: S
8       4096: E                   64: S                         64: S
9       4096: M                   64: S                         64: S
10                                64: S                         64: S
                                  4096: M
11                                64: S                         64: S
                                  4096: M                       0: M
```

## Problem 6
Calculate	the	bisection	bandwidth	for	a	4-ary	3-cube	without	end-around,	but	
where	each	link	is	32-bits	wide	and	clocks	at	800MHz.		Calculate	the	bisection	bandwidth	of	an 8-node	
omega	network	with	64-bit	links	that	clock	at	1.2GHz.

Bisection the shortest line that cuts the network in half.

This is what 4-ary 3-cube without end-around connections looks like:

![image](https://github.com/coolnikitav/coding-lessons/assets/30304422/bac846a6-eed9-41b5-89fa-9ff12b26efd2)

Bisection would have 16 bi-directional connections. Thus, it can transfer 16 * 32 * 800MHz = 4.096 Tbps

8-node omega network:

![image](https://github.com/coolnikitav/coding-lessons/assets/30304422/4ae7085d-41f6-42f2-8bf5-195907406fb2)

Bisection bandwidth is 4 because at any time only 4 links are active. It can transfer 84 * 64 * 1.2GHZ = 307.2 Gb/sec

## Problem 7

## Problem 8

## Problem 9
```
Time      P1:                      P2:                      P3:
1         LW R1, 4(R0)
2                                  LW R1, 16(R0)
3                                                           LW R1, 4(R0)
4                                                           SW R2, 100(R0)
5                                                           LW R4, 104(R0)
6                                  LW R3, 100(R0)
7         SW R1, 0(R0)
8         LW R1, 4100(R0)
9         SW R2, 4100(R0)
10                                 SW R3, 4100(R0)
11                                                          SW R5, 0(R0)
```
```
Time      C1:                      C2:                      C3:                      D: 
1         0: S                                                                       0: S: {P1}
2         0: S                     0: S                                              0: S: {P1, P2}
3         0: S                     0: S                     0: S                     0: S: {P1,P2,P3}
4         0: S                     0: S                     0: S                     0: S: {P1,P2,P3}
                                                            64: M                    64: E: {P3}
5         0: S                     0: S                     0: S                     0: S: {P1,P2,P3}
                                                            64: M                    64: E: {P3}
6         0: S                     0: S                     0: S                     0: S: {P1,P2,P3}
                                   64: S                    64: S                    64: S: {P2,P3}
7         0: M                                                                       0: E: {P1}
                                   64: S                    64: S                    64: S: {P2,P3}
8         4096: S                  64: S                    64: S                    0: I
                                                                                     64: S: {P2,P3}
                                                                                     4096: S: {P1}
9         4096: M                  64: S                    64: S                    0: I
                                                                                     64: S: {P2,P3}
                                                                                     4096: E: {P1}
10                                 64: S                    64: S                    0: I
                                   4096: M                                           64: S: {P2,P3}
                                                                                     4096: E: {P2}
11                                 64: S                    64: S                    0: E: {P3}
                                   4096: M                  0: M                     64: S: {P2,P3}
                                                                                     4096: E: {P2}
```


## Problem 10
I don't understand what its asking.
