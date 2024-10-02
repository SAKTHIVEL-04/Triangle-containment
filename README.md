# Triangle-containment cpp
Problem Statement
You are given the coordinates of 
𝑁
N triangles in a Cartesian plane. Your task is to determine how many of these triangles contain the origin (0, 0) within their interior.

A triangle formed by three points 
𝐴
(
𝑥
1
,
𝑦
1
)
A(x 
1
​
 ,y 
1
​
 ), 
𝐵
(
𝑥
2
,
𝑦
2
)
B(x 
2
​
 ,y 
2
​
 ), and 
𝐶
(
𝑥
3
,
𝑦
3
)
C(x 
3
​
 ,y 
3
​
 ) contains the origin if the area of the triangle formed by its vertices is equal to the sum of the areas of the triangles formed with the origin and the vertices of the triangle.

Input Format
The first line contains a single integer 
𝑁
N (1 ≤ 
𝑁
N ≤ 1000), the number of triangles.
The next 
𝑁
N lines each contain six space-separated integers 
𝑥
1
,
𝑦
1
,
𝑥
2
,
𝑦
2
,
𝑥
3
,
𝑦
3
x 
1
​
 ,y 
1
​
 ,x 
2
​
 ,y 
2
​
 ,x 
3
​
 ,y 
3
​
  
(
−
1
0
6
≤
𝑥
1
,
𝑦
1
,
𝑥
2
,
𝑦
2
,
𝑥
3
,
𝑦
3
≤
1
0
6
)
(−10 
6
 ≤x 
1
​
 ,y 
1
​
 ,x 
2
​
 ,y 
2
​
 ,x 
3
​
 ,y 
3
​
 ≤10 
6
 ), representing the coordinates of the three vertices of the triangle.
Output Format
Output a single integer representing the number of triangles that contain the origin.
Example Input
diff
Copy code
3
1 1 -1 -1 2 0
-1 -1 1 1 -1 1
0 2 2 0 1 1
Example Output
Copy code
1
Explanation
In the example provided:

The first triangle does contain the origin.
The second triangle does not contain the origin.
The third triangle also does not contain the origin. Thus, the output is 1, indicating that only one triangle contains the origin.
Constraints
The input values will always form valid triangles (the points are distinct).
This format clearly presents the problem, input and output requirements, and includes examples to illustrate the expected behavior of the solution.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>

#define X first
#define Y second

using namespace std;

const double EPSILON = 0.00001;
const double ZERO    = 0.00000;

typedef pair<double, double> Point;

bool ContainsOrigin(Point a, Point b, Point c)
{        
    double s = a.Y * c.X - a.X * c.Y + (c.Y - a.Y) * ZERO + (a.X - c.X) * ZERO;
    double t = a.X * b.Y - a.Y * b.X + (a.Y - b.Y) * ZERO + (b.X - a.X) * ZERO;
    
    if((s < ZERO) != (t < ZERO)) return false;
    
    double A = -b.Y * c.X + a.Y * (c.X - b.X) + a.X * (b.Y - c.Y) + b.X * c.Y;
    
    if(A < ZERO)
    {
        s = -s;
        t = -t;
        A = -A;
    }
    return ((s + EPSILON > ZERO) && (t + EPSILON > ZERO) && ((s + t) <= A));
}

int main() 
{
    int N;
    cin >> N;
    
    int ans = 0;
    
    Point origin = {ZERO, ZERO};
    
    for(int i=0; i<N; i++)
    {
        Point a, b, c;
        
        cin >> a.X >> a.Y
            >> b.X >> b.Y 
            >> c.X >> c.Y;
                
        if((a.X == ZERO && b.X == ZERO) || (b.X == ZERO && c.X == ZERO) || (a.X == ZERO && c.X == ZERO)
        || (a.Y == ZERO && b.Y == ZERO) || (b.Y == ZERO && c.Y == ZERO) || (a.Y == ZERO && c.Y == ZERO))
        {
            ans++;
            continue;
        }
        if(ContainsOrigin(a, b, c)) ans++;        
    }
    cout << ans;
    
    return 0;
}

