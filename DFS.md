class Maze():
  def __init__(self,maze):
    self.maze = maze
    self.rows = len(maze)
    self.cols = len(maze[0])
  directions = [(1,0),(-1,0),(0,1),(0,-1)]
  def start(self):
      return (0, 0)
  def goal(self):
      return (self.rows - 1, self.cols - 1)
  def neighbors (self,state):
    row,col = state
    for r,c in self.directions:
      new_row = row + r
      new_col = col + c
      if  0 <= new_row < self.rows and  0<= new_col < self.cols and self.maze[new_row][new_col] != 1:
        yield (new_row,new_col)
def DFS(maze:Maze):
  start = maze.start()
  goal = maze.goal()
  stack = [(start,[])]
  visited = set()
  while stack:
    state,path = stack.pop()
    if state == goal:
      return path + [state]
    visited.add(state)
    for neighbor in maze.neighbors(state):
      if neighbor not in visited:
        stack.append((neighbor,path + [state]))
if __name__ == "__main__":
  maze_grid = [[0, 0, 0, 0, 1, 0, 0],
    [1, 1, 0, 1, 1, 0, 1],
    [0, 0, 0, 0, 0, 0, 0],
    [0, 1, 1, 1, 0, 1, 0],
    [0, 0, 0, 0, 0, 1, 0],
    [1, 0, 1, 1, 0, 0, 0],
    [0, 0, 0, 1, 1, 1, 0]]
  maze = Maze(maze_grid)
  path = BFS(maze)
  print(path)
