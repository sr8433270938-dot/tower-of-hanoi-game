# tower-of-hanoi-game
Built an interactive Tower of Hanoi game using Python Turtle Graphics. Features include user-controlled disk movement, smooth animation, win detection with a “Congrats” message, and sound effects. This project improved my understanding of recursion, logic building, and basic game development concepts.
import turtle

# ===== INPUT =====
n = int(input("Enter number of disks (3-5 best): "))

# ===== SCREEN =====
screen = turtle.Screen()
screen.bgcolor("white")
screen.tracer(0)

t = turtle.Turtle()
t.hideturtle()
t.speed(0)

# message turtle
msg = turtle.Turtle()
msg.hideturtle()
msg.penup()

# ===== SETTINGS =====
rods_pos = [-200, 0, 200]
DISK_HEIGHT = 25

# stacks
rods = [
    list(range(n, 0, -1)),
    [],
    []
]

selected = None

# ===== DRAW RODS =====
def draw_rods():
    t.pensize(5)
    for x in rods_pos:
        t.penup()
        t.goto(x, -150)
        t.pendown()
        t.setheading(90)
        t.forward(300)
    t.pensize(1)

# ===== DRAW DISK =====
def draw_disk(x, y, size):
    width = size * 40

    t.penup()
    t.goto(x - width//2, y)
    t.setheading(0)
    t.pendown()

    if size == 1:
        t.color("black", "purple")
    else:
        t.color("black", "red")

    t.begin_fill()
    for _ in range(2):
        t.forward(width)
        t.left(90)
        t.forward(DISK_HEIGHT)
        t.left(90)
    t.end_fill()

# ===== DRAW ALL =====
def draw_all():
    t.clear()
    draw_rods()

    for i, rod in enumerate(rods):
        for j, disk in enumerate(rod):
            draw_disk(rods_pos[i], -150 + j * DISK_HEIGHT, disk)

    screen.update()

# ===== WIN CHECK =====
def check_win():
    if len(rods[2]) == n:
        msg.clear()
        msg.goto(-140, 120)
        msg.color("dark green")
        top_y = screen.window_height()//2- 50

        msg.goto(0, top_y)
        msg.write("🎉 CONGRATS! YOU WON 🎉",align="center", font=("Arial",20, "bold"))
# ===== MOVE ====d
def move_disk(from_rod, to_rod):
    if not rods[from_rod]:
        return
    if rods[to_rod] and rods[to_rod][-1] < rods[from_rod][-1]:
        return

    rods[to_rod].append(rods[from_rod].pop())
    draw_all()
    check_win()

# ===== KEY CONTROL =====
def press_1():
    global selected
    if selected is None:
        selected = 0
    else:
        move_disk(selected, 0)
        selected = None

def press_2():
    global selected
    if selected is None:
        selected = 1
    else:
        move_disk(selected, 1)
        selected = None

def press_3():
    global selected
    if selected is None:
        selected = 2
    else:
        move_disk(selected, 2)
        selected = None

# ===== START =====
draw_all()

screen.listen()
screen.onkey(press_1, "1")
screen.onkey(press_2, "2")
screen.onkey(press_3, "3")

turtle.done()
