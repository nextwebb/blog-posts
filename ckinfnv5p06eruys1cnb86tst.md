## Concurrency in Golang


A typical system is made up of smaller components and sub-systems that all work together.
Software programs are often made up of much smaller sub-programs as well.


![lucas-favre-4x-67z_TaGo-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1607881454582/JYwuksvZ9.jpeg)

For example, a web server such as [Apache](https://httpd.apache.org/) handles requests made from the web clients and serves up static or dynamic content or HTML web pages as a response. Each request is handled like a small program, in a series of response and request cycles.


![What-is-a-Web-Server.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1607868397792/7pOt3HsCY.webp)


How ideal it'll be for large programs to run their smaller components independently and simultaneously  🤔 !?

> *Making progress on more than one task simultaneously is known as concurrency.*

  [Golang](https://golang.org/), At the language level offers some great concurrency primitives and makes it extremely easy to implement a concurrent system.
Go has rich support for concurrency using 
- Goroutines &
- Channels.

## Goroutines

 Goroutine always works in the background. They are functions executing concurrently with other goroutines in the same thread or set of threads. To create a goroutine we use the keyword ``` go ``` followed by a function invocation:

```
package main

import (
	"time"
	"fmt"
)

func say(s string){
	for i:=0; i < 3; i++ {
		fmt.Println(s)
		time.Sleep(time.Microsecond*100)
	}

} 
func main() {
	start := time.Now()

	go say("Hey")
	go say("There")

	elapsed := time.Since(start)
	fmt.Println(elapsed)

//the program runs concurrently
}

```
Every program contains at least one  Goroutine and that Goroutine is known as the **main Goroutine**. 

This program above consists of three goroutines. 
The first goroutine is implicit, it is the main function itself and all the goroutines are working under the **main Goroutines **.

The second and third goroutine is created when we invoke a function; In this case by calling ``` go say("Hey") & go say("There")```. 

By convention, when a function is invoked our program will execute all the statements within the function and then return to the next line following the invocation.
But with a goroutine, we return immediately to the next line and don't wait for the function to complete.

 We can investigate this concurrency and the program time of execution by setting some delay.

A hack 👨‍💻 I found was to use the ```sleep``` method ``` time.Sleep(time.Second)  ``` . 

 Without this, the program would exit before being given the opportunity to print all the strings  ''hey'' and "there". 

Goroutines are lightweight and we can easily create many of them in our programs. We can modify the above program to run 20 😃goroutines by doing this:

```
func main() {

start := time.Now()

  for i := 0; i < 10; i++ {
    go say("Hey")
    go say("There")

  }

time.Sleep(time.Second)
elapsed := time.Since(start)
fmt.Println(elapsed)

}

```

## Channels
Channels are used for communication and sometimes synchronization between two executing goroutines.
Here is a sample program using channels:

```
package main

import (
	"fmt"
	"time"
)

func main() {
	// Channels are defined using the make function,
	// VariableName:= make(chan value-type)

	ch := make(chan string)

	// this sends data to the channel
	go greeting("Hello", ch)
	for {
		// We can indicate the direction of the data flow by using an arrow ch <- data

              msg := <- ch
		// and v:= <- ch receives the data from the channel!

		fmt.Println(msg)
	}
}

func greeting(s string, ch chan string) {
	for i := 0; i <= 5; i++ {
		//WE SEND A VALUE THROUGH A CHANNEL
		ch <- s
		time.Sleep(100 * time.Millisecond)
	}
	close(ch)
}

```
The channel is created using the ```make ```function and the ```chan``` keyword, it is then passed on to the call to the ``` greeting``` function which accepts a string and a channel of data-type string. The for loop is created in the main channel to receive and print out every incoming message. 

The ```close()``` function indicates Closing a channel; thus no more values will be sent on it. This can be useful to communicate completion to the channel’s receivers.

So using a channel like this synchronizes the two goroutines.

## Conclusion

I briefly talked about the essential concepts in Golang such as goroutines and channels, which are the implementations of concurrency in [Go](https://golang.org/).  I hope this article helps you learn more about [Golang](https://golang.org/) features 😃. 

**I enjoyed writing this article and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.**

Do watch the [Go-Routines](https://www.youtube.com/watch?v=oV9rvDllKEg&t=502s) video by *Rob Pike* and have fun coding in Golang.

Until next time…


## Further Reading
- [Learn More about Go concurrency ](https://tour.golang.org/concurrency/1)

