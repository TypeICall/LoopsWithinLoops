# LoopsWithinLoops
Notes on creating loops within loops<br>

for loops<br>

Syntactical Structure<br>
for(int i = 0; i < limit; i++)<br>
for(variable type initialized at an amount; <br>
    comparision operation that tells the loop when it is time to break the loop and go on to the lines of code after;<br>
    incrementer)<br>

typical loops:<br>

Ascending<br>
for(int i = 0; i < limit; i++) { }<br>
Descending<br>
for(int i = limit; i > 0; i--) { }<br>

Could also change increment as in simple polynomials<br>
Evens = 2n <br>
for(int i = 0; i < limit; i +=2) { }<br>
Odds = 2n + 1 <br>
for(int i = 1; i < limit; i +=2) { }<br>

Could have an increment that is not regularly spaced<br>
int increment = 2;<br>
for(int i = 0; i < limit; i += increment) { increment *=2; }<br>

Could change the type of the variable from int to float.<br>
This goes around a circle sector by sector-<br>
Vector2[] CircumRadii;<br>
int pointCount = 0;<br>
float Sector = (2*Mathf.PI)/ nGon;<br>
float Round = 2 * Mathf.Pi;
for(float i = 0; i <= Round; i += Sector) { CircumRadii[pointCount] = new Vector2(Mathf.sin(i), Mathf.cos(i)); pointCount++; }<br>

This could also be done with a while loop <br>
Vector2[] CircumRadii;<br>
int i = 0;<br>
int nGon; <br>
float Sector = (2*Mathf.PI)/ nGon;<br>
float slices = 0;<br>
while(i <= nGon)<br>
{<br>
CircumRadii[i] = new Vector2(Mathf.sin(slices), Mathf.cos(slices)); }<br>
slices += Sector;<br>
i++; <br>
}<br>


Could also have multiple evaluations in the middle<br>
for(int i = 0; i < 60 && b > 0; i++)<br>

Then we get to having multiple for loops within one another.<br>

Square = where there are n^2 total loops<br>
for(int x = 0; x < n; x++)<br>
  for(int y = 0;y < n; y++)<br>

Rectangular = This is how computer monitor's refresh. where there are x * y loops<br>
Color32[] colors32 = new Color32[ScreenHeightInPixels * ScreenWidthInPixels];<br>
for(int x = 0; x < ScreenHeightInPixels; x++)<br>
  for(int y = 0;y < ScreenWidthInPixels; y++)<br>
    colors32[y,x] = new Color32(R,G,B,A);  // where RGBA are in the range between (0-255) and then 256 goes to 0, 257 goes to 1, and 258 goes to 2, etc.<br>
    OR<br>
    colors[y,x] = new Color(R,G,B,A); (where R,G,B,A are in the range between (0-255)<br>
  
Cubic<br>
for(int x = 0; x < n; x++)<br>
  for(int y = 0; y < n; y++)<br>
    for(int z = 0; z < n; z++)<br>
    
Cuboidal<br>
for(int x = 0; x < height; x++)<br>
  for(int y = 0; y < width; y++)<br>
    for(int z = 0; z < depth; z++)<br>

In an instance where checking x againt y is the same as checking y against x, such as finding the distances between points,<br>
we can simplify the equation<br>
We find the triangular numbers described by the equation<br>
(n * (n - 1)) * 0.5f = (n ^ 2 - n) * 0.5f<br>

// DO NOT COPY PASTE THE FOLLOWING SECTION. IT IS NOT OPTIMIZED. <br>
for(int x = 0; x < n; x++)<br>
  for(int y = x + 1; y < n; y++)<br>
      Distances[x * n + y] = Mathf.Sqrt(vertices[x].x - vertices[y].x + vertices[x].y - vertices[y].y + vertices[x].z - vertices[y].z);<br>
  
 We should remain vigilant where we state "Distances[x * n + y]", as we go from this quadratic polynomial on to cubic or quartic polynomials,<br>
 and we start looking at even more for loops being in for loops, being in for loops, etc. and we start to see that <br>
 2 nested for loops would need [x * n + y]<br>
 3 nested for loops would need [x * n * n  + y * n + z]<br>
 4 nested for loops would need [x * n * n * n + y * n * n + z * n + w]... You get the point.<br>
 
 Imagine that that polynomial would have to be calculated every single time the innermost for loop executed... that would add up to MANY, MANY multiplications<br>
 having to be performed.<br>
 
 Another error that some programmers might make is-<br>
 
 // DO NOT COPY PASTE THE FOLLOWING SECTION. IT IS NOT OPTIMIZED. <br>
 Vector3[] vertices;<br>
Vector3[] Distances = new float[(n * (n - 1)) * 0.5f]<br>
int a = 0, b = 0;
for(int x = 0; x < n; x++)<br>
    a = x * n * n;
  for(int y = 0; y < n; y++)<br>
        b = a + y * n;
      for(int z = 0; y < n; y++)<br>
            Distances[a + b + z] = (Calculations);
            
 This is often seen when programmers are taught mathematics in the form of ax^3 + bx^2 + cx + d = 0. They might think that they could do the indexer by doing these operations in piecemeal, but they could be simplified initializing an INDEXER... (There are also cases where programmers define a and b after the inner for loops are performed, which could also be identified. One would hope the compiler would automatically correct it to prevent flare ups, but that is not guaranteed by any programming language so far as I am aware...) <br>
 
 The INDEXER is simply a variable that also goes in to the for loops and gets incremented by 1 to set the location in memory where bits will be stored.<br>
 
 int index = 0;<br>
 for(int x = 0; x < n; x++)<br>
 {<br>
  for(int y = x + 1; y < n; y++)<br>
  {<br>
      Distances[index] = Mathf.Sqrt(vertices[x].x - vertices[y].x + vertices[x].y - vertices[y].y + vertices[x].z - vertices[y].z);<br>
      index++;<br>
  }<br>
} <br>
As simple as it gets I do believe, saves on electricity, allows the program to execute more quickly, nice.<br>
Lesson: Even though it seems impressive to have polynomials with multiple terms in your code, sometimes all you need to do is just keep adding 1 and that is optimal.<br>


Another common optimization error is when the coder decides to define things in the innermost for loop that could be defined in an outer for loop.
for(int x = 0; x < n; x++)<br>
  for(int y = 0; y < n; y++)<br>
      for(int z = 0; y < n; y++)<br>
            Differences[a + b + z] = (Mathf.Abs(y -z), Mathf.Abs(x - z), Mathf.Abs(x - y))<br>
            
Here the Mathf.Abs(x - y) term could be brought out in order to save some calculations.<br>

int xy;
for(int x = 0; x < n; x++)<br>
  for(int y = 0; y < n; y++)<br>
      xy = Mathf.Abs(x - y);
      for(int z = 0; y < n; y++)<br>
            Differences[a + b + z] = (Mathf.Abs(y -z), Mathf.Abs(x - z), xy)<br><br>
 Scanning over your code to see if some calculations could be brought up out of the inner for loops and defined fewer times is good practice.<br>
 
 
What happens if we continue this pattern?<br>
Well, it does seem that we start moving along Pa....'s Triangle.<br>
From the row of (1,1,1,1...), to the natural numbers (1,2,3,4...) to the triangular numbers (1,3,6,10...) to the tetrahedral numbers(1,4,10,20,...)<br>

I found the tetrahedral numbers while writing a program that find the angles between any three points in space... or at least this is one way of doing it...<br>
This method ensures that there are instances where the same triangles are being checked with reorderings of the vertices.<br>
Vector3[] verts;<br>
Vector3[] Angles = new float[(n * (n + 1) * (n + 2))/ 6];<br>
int index = 0;<br>
for(int x = 0; x < verts.Length; x++)<br>
{<br>
  for(int y = x + 1; y < verts.Length; y++)<br>
  {<br>
    for(int z = y + 1; z < verts.Length; z++)<br>
    {<br>
      Angles[index] = ???;<br>
      index++;<br>
    }<br>
  }<br>
}<br>


What about for loops with a Jagged Array?<br>
They do need to have each ([]  = square bracket) size defined so that a number of bits can ba allocated in memory.<br>
This is a simple multiplication of (2 ^(number of bits of data type) * number of slots allocated in memory)<br>
int[][] numbers = new int[n][]<br>
for(int i = 0; i < n; i++)<br>
{<br>
  numbers[i] = new int[n];<br>
  for(int j = 0; j < n; j++)<br>
  {<br>
    numbers[i][j] = i+j;<br>
  }<br>
}<br>

Multi-Dimensional Arrays?<br>
int[,] numbers = new int[n,n]<br>
for(int i = 0; i < n; i++)<br>
{<br>
  for(int j = 0; j < n; j++)<br>
  {<br>
    numbers[i,j] = i+j;<br>
  }<br>
}<br>
<br>

What happens if you declare too few spaces in memory for the array to fit? <br>
The program will stop running once it reaches the # address in memory that is equal to the number of spots that were designated to be in memory when the array was intialized. <br>
This would look like-<br>
int[] points = new int[4];<br>
for(int x = 0; x < 8; x++)<br>
{<br>
    points[x] = x; <br>
}<br>

When x is incremented up to 4, then the error will be recognized because there were only memory locations (0,1,2,3) created<br>

What happens if you declare too many spaces in memory for the array to fit? <br>
It might seem like a quick fix to just make the number of spaces in memory to be a higher number than you would need, instead of taking the time to find the exact polynomial, but<br>
some problems can arise from that choice. <br>
The program is stored in memory mostly continuously, linearly, as you define each data type, line by line. <br>
There are some breaks where certain keywords will cause the Read to go to other locations in memory, which does cost some time and electricity, but if you are writing a program then it will be mostly contained within a neighborhood and that neighborhood of transistors switched on or off with current flowing through or not flowing through- those neighborhoods will have to be navigated through by the reader. It will take longer in navigating through memory if you have extra, empty memory that needs to be passed by. <br> 

Why not use a list?<br>
Lists have to resize their selves. Because the size of the list is unknown, they get allocated a small number of bits in memory initially, <br>
but then once they reach a ((2 ^ n)- 1) threshold and another term is added, then they will have to change their size. <br>
There are performance gains when we use arrays and tell exactly how much memory there is going to be used for the array before we start adding terms to it.<br>
It is not always the case that we know how big an array will be, so there are many cases where it makes sense to use Lists.<br>

Why use a list? <br>
Lists allow for easier ways of adding and subtracting one or more terms in a single line of code. <br>
When a piece of data is removed from a list, then it will automatically cause the item that was stored after to move in to the now empty spot in memory and then every term thereafter will have their # in memory reduced by how many items were deleted.<br>
This can cause some confusion when looping through the list and trying to sequentially remove items while the list is restructuring it's self.<br>
If one is using the list as a minimum or maximum value in a loop, the size of that list could be changed inside of that loop and therefore seem to move about it's # address in memory. <br>

When to use Lists? <br>
Lists are really helpful when one is not certain how many addresses in memory seem reasonable to designate. <br>
Technically, Lists are not needed if the data is static and unchanging throughout the course of the program's duration.<br>
Lists are more associated with the idea of mutability and Lists allow data to flow in and out with ease. <br>

