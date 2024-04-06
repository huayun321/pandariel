---
weight: 1
bookFlatSection: true
title: "Nana Go Tutorial"
---

# Nana 的 Go Tutorial

```shell
package main

import (
	"fmt"
	"strings"
)

func main() {
	var conferenceName = "Go Conference"
	const conferenceTickets int = 50
	var remainingTickets uint = 50
	var bookings []string

	fmt.Printf("Welcome to %v booking application\n", conferenceName)
	fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
	fmt.Println("Get your tickets here to attend")

	var firstName string
	var lastName string
	var email string
	var userTickets uint

	for {
		fmt.Println("Enter your first name: ")
		fmt.Scan(&firstName)

		fmt.Println("Enter your last name: ")
		fmt.Scan(&lastName)

		fmt.Println("Enter your email address: ")
		fmt.Scan(&email)

		fmt.Println("Enter number of tickets: ")
		fmt.Scan(&userTickets)

		bookings = append(bookings, firstName+" "+lastName)

		var firstNames []string
		for _, v := range bookings {
			firstNames = append(firstNames, strings.Fields(v)[0])
		}

		fmt.Println("Bookings ", firstNames)
		remainingTickets = remainingTickets - userTickets
		fmt.Println("remainingTickets ", remainingTickets)
	}

}
```


### 资源
* [Nana go tutorial](https://www.youtube.com/watch?v=yyUHQIec83I)
