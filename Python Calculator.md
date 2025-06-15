```python
# Import the tkinter library
from tkinter import *
from tkinter.messagebox import showerror

# GUI Interface goes here
root = Tk()
root.title("Tessa\'s Calculator")
root.geometry('270x500')
root.resizable(0, 0)
root.configure(background='dodgerblue4')

# StringVar variables
entry_strvar = StringVar(root)

# First Section of calculator
Label(root, text='TESSA\'S CALCULATOR', font=("Public Sans", 15, "bold"), fg="blanchedalmond", bg="dodgerblue4").place(x=20, y=0)

Label(root, text='Press \'x\' twice to exponentiate', font=("century gothic", 10), fg="white", bg='dodgerblue4').place(x=30, y=30)

eqn_entry = Entry(root, justify=RIGHT, textvariable=entry_strvar, width=27, font=12, state='disabled')
eqn_entry.place(x=10, y=70)


# Creating the functions
def add_text(text, strvar: StringVar):
    strvar.set(f'{strvar.get()}{text}')


def submit(entry: Entry, strvar: StringVar):
    operation = entry.get()
    try:
        strvar.set(f"{strvar.get()}={eval(operation)}")
    except:
        showerror('Error!', 'Please enter a properly defined equation!')


# Button layout: (text, x, y)
button_font = ("Century Gothic", 15)  # Larger font

button_data = [
    ('7', 10, 170),
    ('8', 75, 170),
    ('9', 140, 170),
    ('4', 10, 225),
    ('5', 75, 225),
    ('6', 140, 225),
    ('1', 10, 280),
    ('2', 75, 280),
    ('3', 140, 280),
    ('0', 10, 340)
]

for text, x, y in button_data:
    Button(
        root,
        text=text,
        font=button_font,
        bg='dodgerblue',
        fg='blanchedalmond',
        command=lambda val=text: add_text(val, entry_strvar)
    ).place(x=x, y=y, width=60, height=50)  # Fixed pixel size


# Shared font for all operator buttons
operator_font = ("Century Gothic", 15)

# Operator Buttons: (label shown, text, x, y)
operator_buttons = [
    ('/','/', 205, 170),
    ('x','*', 205, 225),
    ('-','-', 205, 280),
    ('+','+', 205, 340),
    ('.','.', 75, 340),
    ('(','(', 75, 110),
    (')',')', 140, 110)
]

for label, value, x, y in operator_buttons:
    Button(
        root,
        text=label,
        font=button_font,
        bg='aquamarine4',
        fg='blanchedalmond',
        command=lambda val=value: add_text(val, entry_strvar)
    ).place(x=x, y=y, width=60, height=50)  # Fixed pixel size



# Equal, C and AC buttons
special_font = ("Century Gothic", 15)  # Or match your other buttons

# Button layout: (text, x, y, bg_color, command)

special_buttons = [
    ('=', 140, 340, 'cadetblue3', lambda: submit(eqn_entry, entry_strvar)),
    ('C', 205, 110, 'coral4', lambda: entry_strvar.set(entry_strvar.get()[:-1])),
    ('AC', 10, 110, 'coral4', lambda: entry_strvar.set(''))
]

for text, x, y, color, cmd in special_buttons:
    Button(
        root,
        text=text,
        font=special_font,
        bg=color,
        fg="white",
        command=cmd
    ).place(x=x, y=y, width=60, height=50)

# Exit Button
exit_btn = Button(root, text='Close', font=("century gothic", 15), bg='coral4', fg='white', command=lambda: root.destroy())
exit_btn.place(x=10, y=400, width=200, height=40)

# Updating root
root.update()
root.mainloop()
