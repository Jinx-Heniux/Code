# Course 2

## [module-1-activity-bubble-sort-program](https://www.coursera.org/learn/golang-functions-methods/peer/UxoIW/module-1-activity-bubble-sort-program/)

### by me

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func Swap(slice []int, i int, j int) {
	slice[i], slice[j] = slice[j], slice[i]
}

func BubbleSort(slice []int) {
	for i := 0; i < len(slice)-1; i++ {
		for k := i + 1; k < len(slice); k++ {
			if slice[i] > slice[k] {
				// slice[i], slice[k] = slice[k], slice[i]
				Swap(slice, i, k)
			}
		}
	}
}

func main() {

	reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
	fmt.Println("Please enter 10 integers separated by a space:")
	inputStr, err := reader.ReadString('\n') // 读到换行
	// input, err = inputReader.ReadString('\n')
	if err == nil {
		// input = strings.TrimSpace(strings.ToLower(input))
		// fmt.Printf("The input was: %s\n", input)
		inputStr = strings.TrimSpace(inputStr)
		fmt.Printf("inputStr -> %#v\n", inputStr)
	} else {
		return
	}
	// inputStr = strings.TrimSpace(inputStr)
	// fmt.Printf("inputStr -> %#v\n", inputStr)
	// fmt.Printf("%v\n", inputStr)

	// fields := strings.Split(inputStr, " ")
	fields := strings.Fields(inputStr)

	if len(fields) > 10 {
		fmt.Println("Input Error: please enter up to 10 integers!")
		os.Exit(1)
	}
	fmt.Printf("fields -> %#v\n", fields)

	inputIntSlice := make([]int, 0, 10)
	fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	for field := range fields {
		fmt.Printf("field %d -> %#v\n", field, fields[field])
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}
	fmt.Printf("inputIntSlice -> %#v\n", inputIntSlice)

	// lst := []int{2, 5, 13, 7, 21, 1, 3, 2, 0, 9, 6, 7}
	// lst := []int{-10, 2, 5, 13, 7, -1, 21, 1, 3, 2, 0, -5, 9, 6, 7}
	// fmt.Printf("排序前：%v\n", lst)
	BubbleSort(inputIntSlice)
	// fmt.Printf("排序后：%v\n", lst)
	fmt.Printf("Printing the integers out in sorted order, from least to greatest -> %#v\n", inputIntSlice)
}
// by me
```



```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func Swap(slice []int, i int, j int) {
	slice[i], slice[j] = slice[j], slice[i]
}

func BubbleSort(slice []int) {
	for i := 0; i < len(slice)-1; i++ {
		for k := i + 1; k < len(slice); k++ {
			if slice[i] > slice[k] {
				Swap(slice, i, k)
			}
		}
	}
}

func main() {

	reader := bufio.NewReader(os.Stdin)
	fmt.Println("Please enter up to 10 integers separated by a space (e.g. 4 56 3 63 6 -325 35 0 -342 -4):")

	inputStr, err := reader.ReadString('\n')
	if err == nil {
		inputStr = strings.TrimSpace(inputStr)
	} else {
		return
	}

	fields := strings.Fields(inputStr)
	if len(fields) > 10 {
		fmt.Println("Input Error: please enter up to 10 integers!")
		os.Exit(1)
	}

	inputIntSlice := make([]int, 0, 10)

	for field := range fields {
		i, err := strconv.Atoi(fields[field])
		if err != nil {
			fmt.Println("Invalid Input!")
			os.Exit(2)
		}
		inputIntSlice = append(inputIntSlice, i)
	}

	// fmt.Println("Original integers:")
	// fmt.Printf("    -> %v\n", inputIntSlice)
	BubbleSort(inputIntSlice)
	// fmt.Println("Sorted integers:")
	// fmt.Printf("    -> %v\n", inputIntSlice)

	fmt.Printf("Sorted integers: %v\n", inputIntSlice)
}

// by me
```

### by Doug Donohoe

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func main() {
	in := bufio.NewReader(os.Stdin)

	// prompt user for numbers
	fmt.Printf("Please enter up to 10 numbers (separated by spaces): ")
	numsString, err := in.ReadString('\n')
	if err != nil {
		fmt.Printf("Unable to read input: %s", err)
		os.Exit(1)
	}

	// convert to slice of strings
	numsSlice := strings.Split(strings.TrimSpace(numsString), " ")

	// convert to integers
	var nums []int
	for i, num := range numsSlice {
		// no more than 10 elements
		if i == 10 {
			break
		}
		nums = append(nums, StringToInt(num))
	}

	// output info + sort
	fmt.Printf("Input received: %v\n", nums)
	BubbleSort(nums)
	fmt.Printf("After bubble sort: %v\n", nums)

}

func StringToInt(num string) int {
	i, err := strconv.Atoi(num)
	if err != nil {
		fmt.Printf("Error converting '%s' to integer (using 0 instead): %s\n", num, err)
	}
	return i
}

func BubbleSort(slice []int) {
	length := len(slice)
	for i := 0; i < length-1; i++ {
		for j := 0; j < length-i-1; j++ {
			if slice[j] > slice[j+1] {
				Swap(slice, j)
			}
		}
	}
}

func Swap(slice []int, pos int) {
	slice[pos], slice[pos+1] = slice[pos+1], slice[pos]
}


```

### by Ansh joshi

```go
package main

import (
	"fmt"
)

func main() {
	n := 10
	var arr [10]int
	fmt.Println("Enter numbers \n")
	var temp int
	for i := 0; i < n; i++ {
		fmt.Scan(&temp)
		arr[i] = temp
	}

	fmt.Println(arr)
	Bubble(&arr, n)
	fmt.Println("Sorted array is :")
	fmt.Println(arr)
}

func Bubble(arr *[10]int, n int) {
	//i is for number of swaps steps to take
	for i := 0; i < n; i++ {
		for j := 0; j < n-i-1; j++ {
			if arr[j] > arr[j+1] {
				swap(&arr[j], &arr[j+1])
			}
		}
	}
}

func swap(a *int, b *int) {
	var temp int
	temp = *a
	*a = *b
	*b = temp
}

```



## [module-2-activity](https://www.coursera.org/learn/golang-functions-methods/peer/qKrnv/module-2-activity/)

### by me

```go
package main

import (
	"fmt"
)

func GenDisplaceFn(a, v0, s0 float64) func(float64) float64 {
	return func(t float64) float64 {
		return ((a * t * t) / 2.0) + (v0 * t) + s0
	}

}

func main() {

	var (
		a  float64
		v0 float64
		s0 float64
		t  float64
	)
	fmt.Println("Please enter values for acceleration, initial velocity, and initial displacement:")
	fmt.Println("(e.g. 10 2 1):")
	fmt.Scan(&a, &v0, &s0)
	fmt.Println("Please enter a value for time:")
	fmt.Println("(e.g. 3):")
	fmt.Scan(&t)
	fmt.Printf("Your input: \n\tacceleration: %v\n\tinitial velocity: %v\n\tinitial displacement: %v\n\ttime: %v\n", a, v0, s0, t)

	fn := GenDisplaceFn(a, v0, s0)
	fmt.Printf("The result for computing the displacement after %v seconds: %v\n", t, fn(t))

}

```



### by someone else

```go
package main

import "fmt"

func main() {
	var acceleration, velocity, inidisplacement, time float64
	fmt.Print("Enter the value for acceleration : ")
	fmt.Scanln(&acceleration)
	fmt.Print("Enter the value for initial velocity : ")
	fmt.Scanln(&velocity)
	fmt.Print("Enter the value for initial displacement : ")
	fmt.Scanln(&inidisplacement)
	fmt.Print("Enter the time calculating the displacement(in seconds): ")
	fmt.Scanln(&time)
	fn := GenDisplaceFn(acceleration, velocity, inidisplacement)
	fmt.Println("Displacement after", time, "seconds is : ", fn(time))
}

func GenDisplaceFn(acceleration, velocity, displacement float64) func(float64) float64 {
	return func(t float64) float64 {
		return (0.5 * acceleration * (t * t)) + (velocity * t) + displacement
	}
}

```



## [module-3-activity](https://www.coursera.org/learn/golang-functions-methods/peer/Z8tkv/module-3-activity/)

### by me

```go
package main

import "fmt"

type Animal struct {
	food       string
	locomotion string
	noise      string
}

func (a *Animal) Eat() {
	fmt.Println(a.food)
}

func (a *Animal) Move() {
	fmt.Println(a.locomotion)
}

func (a *Animal) Speak() {
	fmt.Println(a.noise)
}

func main() {

	cow := Animal{
		food:       "grass",
		locomotion: "walk",
		noise:      "moo",
	}
	bird := Animal{
		food:       "worms",
		locomotion: "fly",
		noise:      "peep",
	}
	snake := Animal{
		food:       "mice",
		locomotion: "slither",
		noise:      "hsss",
	}

	var (
		name string
		info string
	)

	fmt.Println("Every request from the user must be a single line containing 2 strings. The first string is the name of an animal, either “cow”, “bird”, or “snake”. The second string is the name of the information requested about the animal, either “eat”, “move”, or “speak”. ")
	for {
		fmt.Println("\nPlease enter your request:")
		fmt.Println("(e.g. cow eat)>")
		fmt.Scan(&name, &info)
		fmt.Printf("Your input is animal (%v) | info (%v) \n", name, info)
		switch name {
		case "cow":
			switch info {
			case "eat":
				fmt.Printf("%s -> %s -> ", name, info)
				cow.Eat()
			case "move":
				fmt.Printf("%s -> %s -> ", name, info)
				cow.Move()
			case "speak":
				fmt.Printf("%s -> %s -> ", name, info)
				cow.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		case "bird":
			switch info {
			case "eat":
				fmt.Printf("%s -> %s -> ", name, info)
				bird.Eat()
			case "move":
				fmt.Printf("%s -> %s -> ", name, info)
				bird.Move()
			case "speak":
				fmt.Printf("%s -> %s -> ", name, info)
				bird.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		case "snake":
			switch info {
			case "eat":
				fmt.Printf("%s -> %s -> ", name, info)
				snake.Eat()
			case "move":
				fmt.Printf("%s -> %s -> ", name, info)
				snake.Move()
			case "speak":
				fmt.Printf("%s -> %s -> ", name, info)
				snake.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		default:
			fmt.Println("Wrong Input!")
		}
	}

}


```



### by me

```go
package main

import "fmt"

type Animal struct {
	food       string
	locomotion string
	noise      string
}

func (a *Animal) Eat() {
	fmt.Println(a.food)
}

func (a *Animal) Move() {
	fmt.Println(a.locomotion)
}

func (a *Animal) Speak() {
	fmt.Println(a.noise)
}

func main() {

	cow := Animal{
		food:       "grass",
		locomotion: "walk",
		noise:      "moo",
	}
	bird := Animal{
		food:       "worms",
		locomotion: "fly",
		noise:      "peep",
	}
	snake := Animal{
		food:       "mice",
		locomotion: "slither",
		noise:      "hsss",
	}

	var (
		name string
		info string
	)

	fmt.Println("Every request from the user must be a single line containing 2 strings. The first string is the name of an animal, either “cow”, “bird”, or “snake”. The second string is the name of the information requested about the animal, either “eat”, “move”, or “speak”. ")
	for {
		fmt.Println("\nPlease enter your request (e.g. cow eat):")
		fmt.Print(">")
		fmt.Scan(&name, &info)
		fmt.Printf("%v, %v -> ", name, info)
		switch name {
		case "cow":
			switch info {
			case "eat":
				// fmt.Printf("%s -> %s -> ", name, info)
				cow.Eat()
			case "move":
				// fmt.Printf("%s -> %s -> ", name, info)
				cow.Move()
			case "speak":
				// fmt.Printf("%s -> %s -> ", name, info)
				cow.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		case "bird":
			switch info {
			case "eat":
				// fmt.Printf("%s -> %s -> ", name, info)
				bird.Eat()
			case "move":
				// fmt.Printf("%s -> %s -> ", name, info)
				bird.Move()
			case "speak":
				// fmt.Printf("%s -> %s -> ", name, info)
				bird.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		case "snake":
			switch info {
			case "eat":
				// fmt.Printf("%s -> %s -> ", name, info)
				snake.Eat()
			case "move":
				// fmt.Printf("%s -> %s -> ", name, info)
				snake.Move()
			case "speak":
				// fmt.Printf("%s -> %s -> ", name, info)
				snake.Speak()
			default:
				fmt.Println("Wrong Input!")
			}
		default:
			fmt.Println("Wrong Input!")
		}
	}

}

```



### by Павел Вохмянин

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

type Animal struct {
	food, move, comms string
}

func (an *Animal) Move() {
	fmt.Println(an.move)
}

func (an *Animal) Eat() {
	fmt.Println(an.food)
}

func (an *Animal) Speak() {
	fmt.Println(an.comms)
}

// getTrait method returns supplied trait of the Animal instance
func (an *Animal) getTrait(trait string) {
	switch trait {
	case "eat":
		an.Eat()
	case "move":
		an.Move()
	case "speak":
		an.Speak()
	}
}

// bestiary is a map that describes animal and its traits
// traits must be a pointer to allow us to use methods
type Bestiary map[string]*Animal

// fill the bestiary with information
// bestiary is a string-pointer hash, thus we should call newAnimal to fill it
func (b Bestiary) fill() {
	b["cow"] = newAnimal("grass", "walk", "moo")
	b["bird"] = newAnimal("worms", "fly", "peep")
	b["snake"] = newAnimal("mice", "slither", "hiss")
}

// validateRequest checks whether submitted request contains valid animal and valid trait
func (b Bestiary) validateRequest(animal, trait string) bool {

	fields := map[string]bool{"eat": true, "move": true, "speak": true}
	_, animalExists := b[animal]
	_, traitExists := fields[trait]
	return animalExists && traitExists
}

// newAnimal creates new Animal instance
func newAnimal(food, move, comms string) *Animal {
	var an Animal
	an.food = food
	an.move = move
	an.comms = comms

	return &an
}

// getCommand reads input and makes sure it is in a proper format, or is a quit command
func getCommand(scanner *bufio.Scanner) (string, string, string) {
	fmt.Printf("\n> ")
	scanner.Scan()
	if scanner.Err() != nil {
		return "", "", fmt.Sprintf("Unable to read input [%s]", scanner.Err().Error())
	}

	input := scanner.Text()
	if strings.ToLower(input) == "quit" || strings.ToLower(input) == "q" {
		fmt.Println("Goodbye!")
		os.Exit(0)
	}
	parsedInput := strings.Fields(input)
	if len(parsedInput) != 2 {
		return "", "", fmt.Sprintf("Unable to parse input [%s]", input)
	}

	return parsedInput[0], parsedInput[1], ""
}

// printHelp displays usage
func printHelp() {
	fmt.Println("Request data from bestiary using format <animal> <trait> (enter quit to exit)")
}

func main() {
	bestiary := make(Bestiary)
	bestiary.fill()

	printHelp()
	scanner := bufio.NewScanner(os.Stdin)

	for {
		animal, trait, skipReason := getCommand(scanner)
		if skipReason != "" {
			fmt.Println(skipReason)
			printHelp()
			continue
		}

		if !bestiary.validateRequest(animal, trait) {
			fmt.Printf("Invalid input [%s, %s]\n", animal, trait)
			printHelp()
			continue
		}
		bestiary[animal].getTrait(trait)
	}
}

```



## [module-4-activity](https://www.coursera.org/learn/golang-functions-methods/peer/gurxW/module-4-activity/)

### by me

```go
package main

import (
	"fmt"
	"os"
)

type Animal interface {
	Eat()
	Move()
	Speak()
}

// Define animal type "Cow"
type Cow struct {
	food       string
	locomotion string
	noise      string
}

func (c *Cow) Eat() {
	fmt.Print(c.food)
}
func (c *Cow) Move() {
	fmt.Print(c.locomotion)
}
func (c *Cow) Speak() {
	fmt.Print(c.noise)
}

// Define animal type "Bird"
type Bird struct {
	food       string
	locomotion string
	noise      string
}

func (b *Bird) Eat() {
	fmt.Print(b.food)
}
func (b *Bird) Move() {
	fmt.Print(b.locomotion)
}
func (b *Bird) Speak() {
	fmt.Print(b.noise)
}

// Define animal type "Snake"
type Snake struct {
	food       string
	locomotion string
	noise      string
}

func (s *Snake) Eat() {
	fmt.Print(s.food)
}
func (s *Snake) Move() {
	fmt.Print(s.locomotion)
}
func (s *Snake) Speak() {
	fmt.Print(s.noise)
}

func GetInfo(animalMap map[string]Animal, a_name, a_type_or_info string) {
	value, ok := animalMap[a_name]
	if ok {
		// fmt.Println(value)
		// fmt.Printf("animal -> %#v | %v | %T\n", value, value, value)
		switch a_type_or_info {
		case "eat":
			value.Eat()
		case "move":
			value.Move()
		case "speak":
			value.Speak()
		default:
			fmt.Println("Wrong Input!")
		}
		fmt.Println()
	} else {
		fmt.Println("animal doesn't exist!")
	}

}

func CreateAnimal(animalMap map[string]Animal, a_name, a_type_or_info string) {

	value, ok := animalMap[a_name]
	if ok {
		// fmt.Println(value)
		// panic("animal exists already!")
		fmt.Printf("animal %s exists already! -> %v\n", a_name, value)
	} else {

		switch a_type_or_info {
		case "cow":
			c := &Cow{
				"grass",
				"walk",
				"moo",
			}
			animalMap[a_name] = c
		case "bird":
			b := &Bird{
				"worms",
				"fly",
				"peep",
			}
			animalMap[a_name] = b
		case "snake":
			s := &Snake{
				"mice",
				"slither",
				"hsss",
			}
			animalMap[a_name] = s
		default:
			// panic("Wrong Animal Type!")
			fmt.Println("Wrong Animal Type!")
		}
		fmt.Println("Created it!")
	}
}

func main() {

	var (
		a_command      string
		a_name         string
		a_type_or_info string
	)

	animalMap := make(map[string]Animal)

	fmt.Println("Each “newanimal” command must be a single line containing three strings. The first string is “newanimal”. The second string is an arbitrary string which will be the name of the new animal. The third string is the type of the new animal, either “cow”, “bird”, or “snake”. ")
	fmt.Println()
	fmt.Println("Each “query” command must be a single line containing 3 strings. The first string is “query”. The second string is the name of the animal. The third string is the name of the information requested about the animal, either “eat”, “move”, or “speak”. ")
	fmt.Println("\nCreation command: \n\t\tnewanimal animal_name animal_type (e.g. newanimal mycow cow)")
	fmt.Println("Query command: \n\t\tquery animal_name animal_info (e.g. query cow1 move)")
	fmt.Println("Enter 'q' to exit!")

	for {

		fmt.Println("\n\nPlease type your command:")
		fmt.Print("> ")
		fmt.Scanln(&a_command, &a_name, &a_type_or_info)

		switch a_command {
		case "q":
			os.Exit(0)
		case "newanimal":
			CreateAnimal(animalMap, a_name, a_type_or_info)
		case "query":
			GetInfo(animalMap, a_name, a_type_or_info)
		default:
			fmt.Println("Wrong Command!")
		}

		fmt.Print("\nYour animals: ")
		for k := range animalMap {
			// fmt.Print(k)
			fmt.Printf("%s, ", k)
		}

	}

}

```



### by Rupesh Shah

```go
package main

import (
	"fmt"
	"strings"
)

type Animal interface {
	Eat()
	Move()
	Speak()
}
type Cow struct{ food, locomotion, noise string }

func (c Cow) Eat() {
	fmt.Println("Food " + c.food)
}
func (c Cow) Move() {
	fmt.Println("Locomotion " + c.locomotion)
}
func (c Cow) Speak() {
	fmt.Println("Noise " + c.noise)
}

type Bird struct{ food, locomotion, noise string }

func (b Bird) Eat() {
	fmt.Println("Food " + b.food)
}
func (b Bird) Move() {
	fmt.Println("Locomotion " + b.locomotion)
}
func (b Bird) Speak() {
	fmt.Println("Noise " + b.noise)
}

type Snake struct{ food, locomotion, noise string }

func (s Snake) Eat() {
	fmt.Println("Food " + s.food)
}
func (s Snake) Move() {
	fmt.Println("Locomotion " + s.locomotion)
}
func (s Snake) Speak() {
	fmt.Println("Noise " + s.noise)
}

func main() {
	var input1, input2, input3 string
	var animalMap map[string]Animal
	animalMap = make(map[string]Animal)
	for {
		fmt.Println("Enter command. Valid command formats are as following")
		fmt.Println("For new animal entry - 'newanimal @animalname @animaltype'")
		fmt.Println("For querying animal details - 'query @animalname @action'")
		fmt.Print(">")
		fmt.Scan(&input1, &input2, &input3)
		if input1 == "newanimal" {
			switch strings.ToLower(input3) {
			case "cow":
				animalMap[input2] = Cow{food: "grass", locomotion: "walk", noise: "moo"}
				fmt.Println("Created it!")
			case "bird":
				animalMap[input2] = Bird{food: "worms", locomotion: "fly", noise: "peep"}
				fmt.Println("Created it!")
			case "snake":
				animalMap[input2] = Snake{food: "mice", locomotion: "slither", noise: "hsss"}
				fmt.Println("Created it!")
			default:
				fmt.Println("Invalid animal type, supported types are [cow|bird|snake]")
			}
		} else if input1 == "query" {
			switch strings.ToLower(input3) {
			case "eat":
				animalMap[input2].Eat()
			case "move":
				animalMap[input2].Move()
			case "speak":
				animalMap[input2].Speak()
			default:
				fmt.Println("Invalid action type, supported types are [eat|move|speak]")
			}
		} else {
			fmt.Print("Not a valid command.")
		}
	}
}

```



### by NIKHIL ANAND

```go
package main

import (
	"fmt"
)

type animal struct {
	food       string
	locomotion string
	noise      string
}

type animalInterface interface {
	Eat()
	Move()
	Speak()
}

func (ani animal) Eat() {
	fmt.Println(ani.food)
	// return
}

func (ani animal) Move() {
	fmt.Println(ani.locomotion)
	// return
}

func (ani animal) Speak() {
	fmt.Println(ani.noise)
	// return
}

func main() {
	animalMap := make(map[string]animal)
	animalMap["cow"] = animal{"grass", "walk", "moo"}
	animalMap["bird"] = animal{"worms", "fly", "peep"}
	animalMap["snake"] = animal{"mice", "slither", "hsss"}
	var genralAni animalInterface
	for {
		var command, requestAni, requestType string
		fmt.Print(">")
		fmt.Scan(&command, &requestAni, &requestType)
		if command == "query" {
			genralAni = animalMap[requestAni]
			switch requestType {
			case "eat":
				genralAni.Eat()
			case "move":
				genralAni.Move()
			case "speak":
				genralAni.Speak()
			}
		}
		if command == "newanimal" {
			animalMap[requestAni] = animalMap[requestType]
			fmt.Println("Created it!")
		}
	}
}

```



### by Jassimran Virdi

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

type Animal interface {
	Eat()
	Move()
	Speak()
}

type Cow struct{}

func (a *Cow) Eat() {
	fmt.Println("grass")
}
func (a *Cow) Move() {
	fmt.Println("walk")
}
func (a *Cow) Speak() {
	fmt.Println("moo")
}

type Bird struct{}

func (a *Bird) Eat() {
	fmt.Println("worms")
}
func (a *Bird) Move() {
	fmt.Println("fly")
}
func (a *Bird) Speak() {
	fmt.Println("peep")
}

type Snake struct{}

func (a *Snake) Eat() {
	fmt.Println("mice")
}
func (a *Snake) Move() {
	fmt.Println("slither")
}
func (a *Snake) Speak() {
	fmt.Println("hsss")
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	animals := make(map[string]Animal)
	for {
		fmt.Print("> ")
		input, _ := reader.ReadString('\n')
		s := strings.Split(strings.TrimSpace(input), " ")

		switch s[0] {
		case "newanimal":
			switch s[2] {
			case "cow":
				animals[s[1]] = new(Cow)
			case "bird":
				animals[s[1]] = new(Bird)
			case "snake":
				animals[s[1]] = new(Snake)
			}
			fmt.Println("Created!")
		case "query":
			obj, ok := animals[s[1]]
			if ok {
				switch s[2] {
				case "eat":
					obj.Eat()
				case "move":
					obj.Move()
				case "speak":
					obj.Speak()
				}
			} else {
				fmt.Println("Not found!")
			}
		}
	}
}

```



