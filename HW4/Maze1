import tkinter as tk
from queue import PriorityQueue

class Cell:
    def __init__(self, x, y, is_wall):
        self.x = x
        self.y = y
        self.is_wall = is_wall
        self.parent = None

class MazeGame:
    def __init__(self, root, maze, algorithm):
        self.root = root
        self.maze = maze
        self.algorithm = algorithm
        
        self.rows = len(maze)
        self.cols = len(maze[0])

        self.agent_pos = (0, 0)
        self.goal_pos = (self.rows - 1, self.cols - 1)
        
        self.cells = [[Cell(x, y, maze[x][y] == 1) for y in range(self.cols)] for x in range(self.rows)]
        self.cells[self.agent_pos[0]][self.agent_pos[1]].g = 0
        self.cells[self.agent_pos[0]][self.agent_pos[1]].h = self.heuristic(self.agent_pos)
        self.cells[self.agent_pos[0]][self.agent_pos[1]].f = self.heuristic(self.agent_pos)

        self.cell_size = 75
        self.canvas = tk.Canvas(root, width=self.cols * self.cell_size, height=self.rows * self.cell_size, bg='white')
        self.canvas.pack()

        self.draw_maze()
        self.find_path()

    def draw_maze(self):
        for x in range(self.rows):
            for y in range(self.cols):
                color = 'maroon' if self.maze[x][y] == 1 else 'white'
                self.canvas.create_rectangle(y * self.cell_size, x * self.cell_size, (y + 1) * self.cell_size, (x + 1) * self.cell_size, fill=color)

    def heuristic(self, pos):
        return abs(pos[0] - self.goal_pos[0]) + abs(pos[1] - self.goal_pos[1])

    def find_path(self):
        open_set = PriorityQueue()
        open_set.put((0, self.agent_pos))

        while not open_set.empty():
            current_cost, current_pos = open_set.get()
            current_cell = self.cells[current_pos[0]][current_pos[1]]

            if current_pos == self.goal_pos:
                self.reconstruct_path()
                break

            for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                new_pos = (current_pos[0] + dx, current_pos[1] + dy)

                if 0 <= new_pos[0] < self.rows and 0 <= new_pos[1] < self.cols and not self.cells[new_pos[0]][new_pos[1]].is_wall:
                    new_g = current_cell.g + 1
                    if new_g < self.cells[new_pos[0]][new_pos[1]].g:
                        self.cells[new_pos[0]][new_pos[1]].g = new_g
                        self.cells[new_pos[0]][new_pos[1]].h = self.heuristic(new_pos)
                        self.cells[new_pos[0]][new_pos[1]].f = new_g + self.cells[new_pos[0]][new_pos[1]].h
                        self.cells[new_pos[0]][new_pos[1]].parent = current_cell
                        open_set.put((self.cells[new_pos[0]][new_pos[1]].f, new_pos))

    def reconstruct_path(self):
        current_cell = self.cells[self.goal_pos[0]][self.goal_pos[1]]
        while current_cell.parent:
            x, y = current_cell.x, current_cell.y
            self.canvas.create_rectangle(y * self.cell_size, x * self.cell_size, (y + 1) * self.cell_size, (x + 1) * self.cell_size, fill='green')
            current_cell = current_cell.parent

    def move_agent(self, event):
        pass

def main():
    maze = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
        [0, 1, 1, 1, 0, 1, 1, 1, 1, 1],
        [0, 0, 0, 1, 0, 0, 0, 0, 0, 0],
        [0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
        [0, 1, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0, 1, 1, 1, 1, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 1, 1, 1, 0, 1, 1, 1, 1, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 1, 1, 1, 1, 1, 1, 1, 1, 0]
    ]

    root = tk.Tk()
    root.title("Maze Solver")

    frame = tk.Frame(root)
    frame.pack()

    a_star_frame = tk.Frame(frame)
    a_star_frame.pack(side=tk.LEFT)
    a_star_label = tk.Label(a_star_frame, text="A* Algorithm")
    a_star_label.pack()
    a_star_canvas = tk.Canvas(a_star_frame, width=len(maze[0]) * 75, height=len(maze) * 75, bg='white')
    a_star_canvas.pack()
    a_star_game = MazeGame(a_star_canvas, maze, "A*")

    gbfs_frame = tk.Frame(frame)
    gbfs_frame.pack(side=tk.RIGHT)
    gbfs_label = tk.Label(gbfs_frame, text="Greedy Best-First Algorithm")
    gbfs_label.pack()
    gbfs_canvas = tk.Canvas(gbfs_frame, width=len(maze[0]) * 75, height=len(maze) * 75, bg='white')
    gbfs_canvas.pack()
    gbfs_game = MazeGame(gbfs_canvas, maze, "Greedy Best-First")

    root.mainloop()

if __name__ == "__main__":
    main()
