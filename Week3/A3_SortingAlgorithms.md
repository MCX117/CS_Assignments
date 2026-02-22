# Assignment 3 - Sorting Algorithms
1. The time complexity of an algorithm $4N + 16 steps$ is <mark> **O(N)** </mark> steps because, in the long run, constants don't matter as much as exponents.
2. An algorithm $2N^2$ is <mark> **O(N<sup>2</sup>)** </mark>. Again, in terms of time complexity, the Big O cares only about the bigger picture. As N gets big, constants don't matter as much as exponents.
3. The following is **2N steps** with a time complexity of <mark> **O(N)** </mark>. See comments below.
   ```ruby
   def double_then_sum(array) 
    	doubled_array = []
    
    	array.each do |number| 
    		doubled_array << number *= 2    // 1 step per element doubled. Therefore, N steps.
    	end
    
    	sum = 0
    
    	doubled_array.each do |number| 
    		sum += number                   // 1 step per element added. Therefore, N steps, again. Overall, 2N.
    	end
    	return sum                        // 2N = O(N) time complexity
    end
   ```

4. The following code is **3N steps** and <mark> **O(N)** </mark> in time complexity.
   ```
   def multiple_cases(array) 
    	array.each do |string|
    		puts string.upcase        // N elements. every element upcase is 1 step. Thus, N steps.
    		puts string.downcase      // Every element downcase is 1 step. Thus, N steps. Overall, 2N steps.
    		puts string.capitalize    // Every element capitalied is 1 step. Thus, N steps. Overall, 3N steps.
    	end 
   end
   ```

5. The following code is **(N<sup>2</sup>/2)** with a time coplexity of <mark> **O(N<sup>2</sup>)** </mark>.
  ```
   def every_other(array) 
  	array.each_with_index do |number, index|    // Every index in the array. so N
  		if index.even?                            // if even... N/2 steps.
  			array.each do |other_number|          
              	puts number + other_number      // 1 step per addition. Runs full inner N.
  			end                                     // N/2 * N = (N^2)/2
  		end
  	end 
  end
  ````
## Video

**Note:** Video may get a little blurry. My video quality on Zoom drops on Linux (using wrapper).

https://github.com/user-attachments/assets/6fd3ef98-f475-441f-a395-5ef695b74c7a







