---
weight: 1
bookFlatSection: true
title: "Nana Go Tutorial"
---

# Nana 的 Go Tutorial

main.go
```shell
package main

import (
	"booking/helper"
	"fmt"
	"time"
)

var conferenceName = "Go Conference"

const conferenceTickets int = 50

var remainingTickets uint = 50
var bookings = make([]UserData, 0)

type UserData struct {
	firstname       string
	lastname        string
	email           string
	numberOfTickets uint
}

//var wg = sync.WaitGroup{}

func main() {

	greetUsers(conferenceName, conferenceTickets, remainingTickets)

	for {
		firstName, lastName, email, userTickets := getUserInput()

		isValidName, isValidEmail, isValidTicketNumber := helper.ValidateUserInput(firstName, lastName, email, userTickets, remainingTickets)

		if isValidName && isValidEmail && isValidTicketNumber {
			bookings = bookTicket(bookings, firstName, lastName, remainingTickets, userTickets, email)

			//wg.Add(1)
			go sendTicket(UserData{firstname: firstName, lastname: lastName, email: email, numberOfTickets: userTickets})

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
	//wg.Wait()
}

func greetUsers(conferenceName string, conferenceTickets int, remainingTickets uint) {
	fmt.Printf("Welcome to %v booking application\n", conferenceName)
	fmt.Printf("We have total of %v tickets and %v are still available.\n", conferenceTickets, remainingTickets)
	fmt.Println("Get your tickets here to attend")
}

func getFirstNames(bookings []UserData) []string {
	var firstNames []string
	for _, v := range bookings {
		firstNames = append(firstNames, v.firstname)
	}
	fmt.Println("dsf", firstNames)

	return firstNames
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

func bookTicket(bookings []UserData, firstName, lastName string, remainingTickets, userTickets uint, email string) []UserData {

	userData := UserData{
		firstname:       firstName,
		lastname:        lastName,
		email:           email,
		numberOfTickets: userTickets,
	}

	bookings = append(bookings, userData)
	fmt.Println("Bookings ", bookings)

	firstNames := getFirstNames(bookings)
	fmt.Println("firstNames ", firstNames)

	remainingTickets = remainingTickets - userTickets
	fmt.Println("remainingTickets ", remainingTickets)

	return bookings
}

func sendTicket(data UserData) {
	time.Sleep(10 * time.Second)
	ticket := fmt.Sprintf("%v ticket for %v %v", data.numberOfTickets, data.firstname, data.lastname)
	fmt.Println("########")
	fmt.Printf("sending ticket:\n %v \n to email address %v \n", ticket, data.email)
	fmt.Println("########")
	//wg.Done()
}

```
helper.go

```shell
package helper

import "strings"

func ValidateUserInput(firstName, lastName, email string, userTickets, remainingTickets uint) (bool, bool, bool) {
	isValidName := len(firstName) >= 2 && len(lastName) >= 2
	isValidEmail := strings.Contains(email, "@")
	isValidTicketNumber := userTickets > 0 && userTickets <= remainingTickets

	return isValidName, isValidEmail, isValidTicketNumber
}

```

### 资源
* [Nana go tutorial](https://www.youtube.com/watch?v=yyUHQIec83I)
