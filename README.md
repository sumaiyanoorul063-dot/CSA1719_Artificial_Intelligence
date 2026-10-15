1) Write the python program to solve 8-Puzzle problem

from heapq import heappush, heappop

goal = (1, 2, 3,
        4, 5, 6,
        7, 8, 0)

def heuristic(state):
    return sum(
        1 for i, tile in enumerate(state)
        if tile != 0 and tile != goal[i]
    )

def get_neighbors(state):
    neighbors = []
    zero = state.index(0)
    moves = [-3, 3, -1, 1]

    for move in moves:
        new = zero + move

        if 0 <= new < 9:
            if move == -1 and zero % 3 == 0:
                continue
            if move == 1 and zero % 3 == 2:
                continue

            temp = list(state)
            temp[zero], temp[new] = temp[new], temp[zero]
            neighbors.append(tuple(temp))

    return neighbors

def solve(start):
    pq = []
    heappush(pq, (heuristic(start), 0, start, []))
    visited = set()

    while pq:
        f, g, state, path = heappop(pq)

        if state == goal:
            return path + [state]

        if state in visited:
            continue
        visited.add(state)

        for neighbor in get_neighbors(state):
            if neighbor not in visited:
                heappush(
                    pq,
                    (g + 1 + heuristic(neighbor),
                     g + 1, neighbor, path + [state])
                )

    return None

start = (1, 2, 3,
         4, 0, 6,
         7, 5, 8)

solution = solve(start)

if solution:
    for step, state in enumerate(solution):
        print("Step", step)
        for i in range(0, 9, 3):
            print(state[i:i+3])
        print()
else:
    print("No solution found")

2) Write the python program to solve 8-Queen problem

N = 8

def print_board(board):
    for row in board:
        print(" ".join("Q" if x else "." for x in row))

def is_safe(board, row, col):
    for i in range(row):
        if board[i][col]:
            return False

        if col - (row - i) >= 0 and board[i][col - (row - i)]:
            return False

        if col + (row - i) < N and board[i][col + (row - i)]:
            return False

    return True

def solve(board, row):
    if row == N:
        return True

    for col in range(N):
        if is_safe(board, row, col):
            board[row][col] = 1

            if solve(board, row + 1):
                return True

            board[row][col] = 0

    return False

board = [[0 for _ in range(N)] for _ in range(N)]

if solve(board, 0):
    print("Solution:")
    print_board(board)
else:
    print("No solution exists")
3)Write the python program for Water Jug Problem

from collections import deque

def water_jug(capacity1, capacity2, target):
    queue = deque([(0, 0)])
    visited = set()

    while queue:
        x, y = queue.popleft()

        if (x, y) in visited:
            continue

        visited.add((x, y))
        print(x, y)

        if x == target or y == target:
            print("Target reached!")
            return

        states = [
            (capacity1, y),          # Fill jug 1
            (x, capacity2),          # Fill jug 2
            (0, y),                  # Empty jug 1
            (x, 0),                  # Empty jug 2
            (x - min(x, capacity2-y), y + min(x, capacity2-y)),
            (x + min(y, capacity1-x), y - min(y, capacity1-x))
        ]

        for state in states:
            if state not in visited:
                queue.append(state)

    print("Target cannot be reached")

water_jug(4, 3, 2)
4)Write the python program for Cript-Arithmetic problem
from itertools import permutations

def cryptarithmetic():
    letters = "SENDMORY"

    for digits in permutations(range(10), 8):
        d = dict(zip(letters, digits))

        if d['S'] == 0 or d['M'] == 0:
            continue

        SEND = 1000*d['S'] + 100*d['E'] + 10*d['N'] + d['D']
        MORE = 1000*d['M'] + 100*d['O'] + 10*d['R'] + d['E']
        MONEY = 10000*d['M'] + 1000*d['O'] + 100*d['N'] + 10*d['E'] + d['Y']

        if SEND + MORE == MONEY:
            print("SEND =", SEND)
            print("MORE =", MORE)
            print("MONEY =", MONEY)
            print("Solution found!")
            return

    print("No solution found.")

cryptarithmetic()
5)Write the python program for Missionaries Cannibal problem
from collections import deque

def missionaries_cannibals():
    start = (3, 3, 1)
    goal = (0, 0, 0)

    moves = [(1, 0), (2, 0), (0, 1), (0, 2), (1, 1)]

    queue = deque([(start, [start])])
    visited = {start}

    while queue:
        state, path = queue.popleft()
        m, c, boat = state

        if state == goal:
            print("Solution path:")
            for step in path:
                print(step)
            return

        for dm, dc in moves:
            if boat == 1:
                new_state = (m - dm, c - dc, 0)
            else:
                new_state = (m + dm, c + dc, 1)

            nm, nc, nb = new_state

            if not (0 <= nm <= 3 and 0 <= nc <= 3):
                continue

            mr = 3 - nm
            cr = 3 - nc

            if (nm > 0 and nc > nm) or (mr > 0 and cr > mr):
                continue

            if new_state not in visited:
                visited.add(new_state)
                queue.append((new_state, path + [new_state]))

    print("No solution found.")

missionaries_cannibals()
6)Write the python program for Vacuum Cleaner problem
def vacuum_cleaner():
    rooms = {'A': 1, 'B': 1}
    position = 'A'

    print("Initial State:", rooms)

    while 1 in rooms.values():
        if rooms[position] == 1:
            print("Room", position, "is dirty. Cleaning...")
            rooms[position] = 0
        else:
            print("Room", position, "is already clean.")

        if position == 'A':
            position = 'B'
        else:
            position = 'A'

    print("Final State:", rooms)
    print("All rooms are clean!")

vacuum_cleaner()
7)Write the python program to implement BFS.
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])

    while queue:
        node = queue.popleft()

        if node not in visited:
            print(node, end=" ")
            visited.add(node)

            for neighbor in graph[node]:
                if neighbor not in visited:
                    queue.append(neighbor)

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

print("BFS Traversal:")
bfs(graph, 'A')
8)Write the python program to implement DFS.
def dfs(graph, node, visited):
    if node not in visited:
        print(node, end=" ")
        visited.add(node)

        for neighbor in graph[node]:
            dfs(graph, neighbor, visited)

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

print("DFS Traversal:")
visited = set()
dfs(graph, 'A', visited)
9)Write the python to implement Travelling Salesman Problem
from itertools import permutations

def tsp(graph, start):
    cities = list(graph.keys())
    cities.remove(start)

    min_cost = float('inf')
    best_path = None

    for path in permutations(cities):
        route = [start] + list(path) + [start]
        cost = 0

        for i in range(len(route) - 1):
            cost += graph[route[i]][route[i + 1]]

        if cost < min_cost:
            min_cost = cost
            best_path = route

    print("Shortest Path:", " -> ".join(best_path))
    print("Minimum Cost:", min_cost)

graph = {
    'A': {'A': 0, 'B': 10, 'C': 15, 'D': 20},
    'B': {'A': 10, 'B': 0, 'C': 35, 'D': 25},
    'C': {'A': 15, 'B': 35, 'C': 0, 'D': 30},
    'D': {'A': 20, 'B': 25, 'C': 30, 'D': 0}
}

tsp(graph, 'A')
10)Write the python program to implement A* algorithm.
import heapq

def astar(graph, heuristic, start, goal):
    queue = [(heuristic[start], 0, start, [start])]
    visited = set()

    while queue:
        f, cost, node, path = heapq.heappop(queue)

        if node == goal:
            print("Shortest Path:", " -> ".join(path))
            print("Total Cost:", cost)
            return

        if node in visited:
            continue

        visited.add(node)

        for neighbor, distance in graph[node].items():
            if neighbor not in visited:
                new_cost = cost + distance
                new_f = new_cost + heuristic[neighbor]
                heapq.heappush(
                    queue,
                    (new_f, new_cost, neighbor, path + [neighbor])
                )

    print("No path found.")

graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'D': 2, 'E': 5},
    'C': {'E': 1},
    'D': {'G': 5},
    'E': {'G': 3},
    'G': {}
}

heuristic = {
    'A': 7,
    'B': 6,
    'C': 4,
    'D': 4,
    'E': 2,
    'G': 0
}

astar(graph, heuristic, 'A', 'G')
