---
weight: 1
bookFlatSection: true
title: 广度优先搜索
---

# 广度优先搜索

```shell
package main

import "fmt"

type GraphMap map[string][]string

func main() {
	var graphMap = make(GraphMap, 0)
	graphMap["you"] = []string{"alice", "bob", "claire"}
	graphMap["bob"] = []string{"anuj", "peggy"}
	graphMap["alice"] = []string{"peggy"}
	graphMap["claire"] = []string{"tom", "johnny"}
	graphMap["anuj"] = []string{}
	graphMap["peggy"] = []string{}
	graphMap["tom"] = []string{}
	graphMap["johnny"] = []string{}

	queue := graphMap["you"]
	breadthFirstSearch(queue, graphMap)
}

func personIsTom(p string) bool {
	return p == "tom"
}

func breadthFirstSearch(queue []string, graph GraphMap) {
	searched := make(map[string]bool)

	for {
		if len(queue) > 0 {
			var person string
			person, queue = queue[0], queue[1:]
			fmt.Println(person)
			if !searched[person] {
				if personIsTom(person) {
					fmt.Printf("%s is already in the queue for you.\n", person)
					break
				} else {
					queue = append(queue, graph[person]...)
					fmt.Println(queue)
					searched[person] = true
					fmt.Println(searched)
				}
			}
		} else {
			fmt.Println("Not found in search queue")
			break
		}
	}
}

```