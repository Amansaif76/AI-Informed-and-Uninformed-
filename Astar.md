import heapq

class Maze:
    def __init__(self, maze):
        self.maze = maze
        self.rows = len(maze)
        self.cols = len(maze[0])

    def start(self):
        return (0, 0)

    def goal(self):
        return (self.rows - 1, self.cols - 1)

    directions = [(1,0), (0,1), (-1,0), (0,-1)]

    def neighbours(self, state):
        r, c = state
        for dr, dc in self.directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < self.rows and 0 <= nc < self.cols and self.maze[nr][nc] != 1:
                yield (nr, nc)

    def manhattan(self, a, b):
        return abs(a[0] - b[0]) + abs(a[1] - b[1])


def Astar(maze: Maze):
    start = maze.start()
    goal = maze.goal()

    visited = set()
    priority_queue = []

    g = 0
    h = maze.manhattan(start, goal)
    f = g + h

    heapq.heappush(priority_queue, (f, g, start, [start]))

    while priority_queue:
        f, g, state, path = heapq.heappop(priority_queue)

        if state == goal:
            return path

        if state in visited:
            continue

        visited.add(state)

        for neighbour in maze.neighbours(state):
            if neighbour not in visited:
                ng = g + 1
                nh = maze.manhattan(neighbour, goal)
                nf = ng + nh
                heapq.heappush(priority_queue, (nf, ng, neighbour, path + [neighbour]))

    return None

maze_layout = [
        [0,0,0,0,1,0,0,0],
        [1,1,0,1,1,0,1,0],
        [0,0,0,0,0,0,1,0],
        [0,1,1,1,1,0,1,0],
        [0,0,0,0,0,0,1,0],
        [0,1,1,1,1,1,1,0],
        [0,0,0,0,0,0,0,0],
        [1,1,1,1,1,1,0,0],
    ]

maze = Maze(maze_layout)
path = Astar(maze)
print(path)
