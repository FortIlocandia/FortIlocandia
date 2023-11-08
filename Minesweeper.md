
import itertools
import random as rand
from string import ascii_uppercase

# Function to generate mine coordinates
def generate_mines():
    return [ascii_uppercase[a] + str(b + 1) for a, b in rand.sample(list(itertools.product(range(7), repeat=2)), k=8)]

# Function to create the Minesweeper grid
def create_minesweeper_grid():
    grid = [['.' for _ in range(7)] for _ in range(7)]

# Place mines on the grid
    mine_coords = generate_mines()

    for coord in mine_coords:
     row = ord(coord[0]) - ord('A')
     col = int(coord[1]) - 1
     grid[row][col] = '!'

    return grid

def (grid, row col) ():
    if grid[row][col] == '!':
        grid[row][col] = '.'

# Function to display the Minesweeper grid with coordinates
def display_minesweeper_grid(grid):

# Display for the game
    print('~' * 30)
    print('    M I N E S W E E P E R ')
    print('~' * 30)

    print('_'* 30)
    header_row = '   ' + '   '.join('ABCDEFG')
    print(header_row)
    print('_' * 30)


    for i, row in enumerate(grid):
        print(str(i+1) + '   ' + '   '.join(row))
        print('_' * 30)

# Function to check if a cell is valid
def is_valid_cell(cell):
    return len(cell) == 2 and cell[0] in ascii_uppercase[:7] and cell[1].isdigit() and 1 <= int(cell[1]) <= 7


# Function to update the game state by revealing a cell
def reveal_cell(grid, cell):
    row = ord(cell[0]) - ord('A')
    col = int(cell[1]) - 1

    if grid[row][col] == '!':
        return 'lose'  # Player loses if they reveal a mine
    else:
        count = 0
        for dr in [-1, 0, 1]:
            for dc in [-1, 0, 1]:
                r, c = row + dr, col + dc
                if 0 <= r < 7 and 0 <= c < 7 and grid[r][c] == '!':
                    count += 1
        grid[row][col] = str(count) if count > 0 else ' '
        return 'continue'  # Player can continue playing

# Function to check if the player has won the game
def has_won(grid):
    for row in grid:
        for cell in row:
            if cell == '.':
                return False
    return True

# Create the Minesweeper grid
minesweeper_grid = create_minesweeper_grid()

# Main game loop
game_over = False
while not game_over:
    display_minesweeper_grid(minesweeper_grid)
    print('')
    cell = input("Enter a coordinate to be opened [A-G] [1-7]: ")
    print('')

    if is_valid_cell(cell):
        result = reveal_cell(minesweeper_grid, cell)

        if result == 'lose':
            game_over = True
            display_minesweeper_grid(minesweeper_grid)
            print('')
            print("You lose! There was a mine at", cell)
        elif has_won(minesweeper_grid):
            game_over = True
            display_minesweeper_grid(minesweeper_grid)
            print('')
            print("Congartulations, You win! All safe cells are revealed.")
    else:
        print("Invalid coordinate. Please enter a valid coordinate ([A-G] [1-7]).")

# End of the game
