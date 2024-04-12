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

var conferenceName = "Go Conference"

const conferenceTickets int = 50

var remainingTickets uint = 50
var bookings []string

func main() {

	greetUsers(conferenceName, conferenceTickets, remainingTickets)

	for {
		firstName, lastName, email, userTickets := getUserInput()

		isValidName, isValidEmail, isValidTicketNumber := validateUserInput(firstName, lastName, email, userTickets, remainingTickets)

		if isValidName && isValidEmail && isValidTicketNumber {
			bookings = bookTicket(bookings, firstName, lastName, remainingTickets, userTickets)

			firstNames := getFirstNames(bookings)
			fmt.Printf("The first names of bookings are %v \n", firstNames)

			if remainingTickets == 0 {
				fmt.Println("Our conference is booked out. Come back next year.")
				break
			}
		} else {
			fmt.Printf("Your input data is invalid, try again. \n")
		}
	}

}

func greetUsers(conferenceName string, conferenceTickets int, remainingTickets uint) {
	fmt.Printf("Welcome to %v booking application\n", conferenceName)
	fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
	fmt.Println("Get your tickets here to attend")
}

func getFirstNames(bookings []string) []string {
	var firstNames []string
	for _, v := range bookings {
		firstNames = append(firstNames, strings.Fields(v)[0])
	}
	fmt.Println("dsf", firstNames)

	return firstNames
}

func validateUserInput(firstName, lastName, email string, userTickets, remainingTickets uint) (bool, bool, bool) {
	isValidName := len(firstName) >= 2 && len(lastName) >= 2
	isValidEmail := strings.Contains(email, "@")
	isValidTicketNumber := userTickets > 0 && userTickets <= remainingTickets

	return isValidName, isValidEmail, isValidTicketNumber
}

func getUserInput() (string, string, string, uint) {
	var firstName string
	var lastName string
	var email string
	var userTickets uint

	fmt.Println("Enter your first name: ")
	fmt.Scan(&firstName)

	fmt.Println("Enter your last name: ")
	fmt.Scan(&lastName)

	fmt.Println("Enter your email address: ")
	fmt.Scan(&email)

	fmt.Println("Enter number of tickets: ")
	fmt.Scan(&userTickets)

	return firstName, lastName, email, userTickets
}

func bookTicket(bookings []string, firstName, lastName string, remainingTickets, userTickets uint) []string {
	bookings = append(bookings, firstName+" "+lastName)

	firstNames := getFirstNames(bookings)
	fmt.Println("Bookings ", firstNames)

	remainingTickets = remainingTickets - userTickets
	fmt.Println("remainingTickets ", remainingTickets)

	return bookings
}

```


### 资源
* [Nana go tutorial](https://www.youtube.com/watch?v=yyUHQIec83I)
