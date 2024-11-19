##### Max or Min element of a vector
These functions return an iterator.. so we have to use `*` to access the values.
```cpp
int max_ele = *max_element(v.begin(), v.end());

int min_ele = *min_element(v.begin(), v.end());
```

##### Converting single digit to char
```cpp
char aChar = '0' + i;

// OR
char digits[] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9' };  
char aChar = digits[i];
```

##### Converting int to string
```cpp
int num1 = 21;
// Converting int to string
string str1 = to_string(num1); 
```

##### Converting string to int
```cpp
int stoi(string str, size_t position = 0, int base = 10);

int num2=stoi(str1);
```

##### 2-Dimensional Map
```cpp
map<int, map<int,int>> m;

m[lvl][height]=root->data; //inserting values

for(auto x : m){
	for(auto y: x.second)
		res.push_back(y.second);
}    //accessing values
```

##### Custom Comparison Functions
```cpp
struct Person{
    int age;
    float ht;
    Person(int a , float h){
        age=a;
        ht=h;
    }
};
struct myCmp{
    bool operator()( Person const &p1, Person const &p2){
        return p1.ht<p2.ht;
    }
};

int main(){
    priority_queue<Person, vector< Person> , myCmp > pq;
    
}
```
**OR**
```cpp
static bool myCmp( Job j1 , Job j2 ){
	return j1.profit > j2.profit;
}
```

##### Initializing an unordered set using a vector
```cpp
 unordered_set<string> New_set(old_arr.begin(), old_arr.end());
```

##### Extracting from string using a stream
- *Stringstream (`stringstream`)*:
    
    - The `stringstream` allows you to treat a string like a stream (similar to `cin`), which means you can use the extraction operator (`>>`) to read data from it sequentially.
- *Extraction Process*:
    - The `>>` operator reads from the `stringstream` until it encounters a character that doesn't fit the type it's trying to extract. For an integer, it will read all consecutive digits that make up the number, regardless of whether it's a single-digit or multi-digit number.
```cpp
queue<Fraction> convert(string& expression) {
        queue<Fraction> fraction;
        stringstream ss(expression);
        char op;
        int num, denom;

        while (ss>>num>>op>>denom) {
            fraction.push(Fraction(num,denom));
        //    cout<<num<<","<<op<<denom<<endl;
        }
        return fraction;
    }
```
#### Algorithms

##### Smallest Divisor of a number
```cpp
    int smallestDivisor(int n) {
        // if divisible by 2
        if (n % 2 == 0)
            return 2;

        // iterate from 3 to sqrt(n)
        for (int i = 3; i * i <= n; i += 2) {
            if (n % i == 0)
                return i;
        }
        return n;
    }
```

##### Reversing and Rotating an Array
```cpp
void reverse(int arr[],int low, int high){
	while(low<high){
		swap(arr[low],arr[high]);
		low++;
		high--;
	}
}
    
void rotate( int arr[] , int d, int n){
	if(d==n) return;
	
	if(d>n) 
		d=d%n;
	reverse(arr,0,d-1);
	reverse(arr,d,n-1);
	reverse(arr,0,n-1);
}
```

##### GCD of two numbers
```cpp
int findGcd(int num1, int num2) {
	if (num2 == 0) {
		return num1;
	}
	return findGcd(num2, num1 % num2);
}

```

##### Checking and finding overlap between two intervals
```cpp
bool checkOverlap(int s1, int e1, int s2, int e2) {
	return max(s1, s2) < min(e1, e2);
}

pair<int,int> findOverlap(int s1, int e1, int s2, int e2) {
	return {max(s1, s2), min(e1, e2)};
}
```
#### Notes
- `int` can hold values up to 32 bits.  So that makes the max value 2$^{31}$
- Number of digits in a number `n` is ( log$_{10}$ n + 1 )