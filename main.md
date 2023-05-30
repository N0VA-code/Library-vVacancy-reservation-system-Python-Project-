```md
<Pyhton Programming Toy Project>
Create a library seat reservation program using the Python language with the tkinter library for GUI implementation. When the 'Seat Reservation Status' button is clicked, the program should display the status of the seats; if a seat is occupied, it should be displayed in red, and if it is available, it should be displayed in blue.
When making a reservation, users should be asked to input their name. Additionally, they should also provide a password upon reservation, which will be used to confirm their identity when they want to cancel the reservation.
The reservation should be automatically cancelled 2 hours after the reservation is made.
```
```py
import tkinter as tk
from tkinter import messagebox
from tkinter import simpledialog

class Library:
    def __init__(self, master):
        self.master = master
        self.master.title('도서관 자리 예약 시스템')

        self.status = ['available'] * 40
        self.names = [''] * 40
        self.passwords = [''] * 40
        self.buttons = []

        for i in range(40):
            button = tk.Button(self.master, text=str(i+1), command=lambda i=i: self.reserve_or_cancel(i), width=10, height=2)
            button.grid(row=i//8, column=i%8)
            self.buttons.append(button)

        self.update_buttons()

        tk.Button(self.master, text="자리 예약현황", command=self.show_status).grid(row=5, column=4, columnspan=2)

    def reserve_or_cancel(self, seat):
        if self.status[seat] == 'available':
            self.reserve(seat)
        else:
            self.cancel(seat)

    def reserve(self, seat):
        name = simpledialog.askstring("이름 입력", "이름을 입력하세요")
        if name:
            password = simpledialog.askstring("비밀번호 입력", "4자리 비밀번호를 입력하세요", show="*")
            if password and len(password) == 4:
                self.status[seat] = 'occupied'
                self.names[seat] = name
                self.passwords[seat] = password
                messagebox.showinfo("예약 완료", f"{seat+1}번 자리가 {name}님에게 예약되었습니다.")
                # 자리를 2분 후에 자동으로 비웁니다. 실제로 2시간 후에 예약을 취소하려면 7200000을 사용하세요.
                self.master.after(120000, self.clear_reservation, seat)
            else:
                messagebox.showwarning("예약 실패", "4자리 비밀번호를 입력하지 않았습니다. 다시 시도하세요.")
        else:
            messagebox.showwarning("예약 실패", "이름을 입력하지 않았습니다. 다시 시도하세요.")
        self.update_buttons()

    def cancel(self, seat):
        password = simpledialog.askstring("비밀번호 입력", "비밀번호를 입력하세요", show="*")
        if password and password == self.passwords[seat]:
            self.clear_reservation(seat)
        else:
            messagebox.showwarning("예약 취소 실패", "비밀번호가 일치하지 않습니다.")
        self.update_buttons()

    def clear_reservation(self, seat):
        self.status[seat] = 'available'
        self.names[seat] = ''
        self.passwords[seat] = ''

    def show_status(self):
        for i, status in enumerate(self.status):
            print(f"자리 {i+1}: {'예약 가능' if status == 'available' else '예약됨'}")
            

    def update_buttons(self):
        for i in range(40):
            if self.status[i] == 'available':
                self.buttons[i].config(bg='blue', text=str(i+1))
            else:
                self.buttons[i].config(bg='red', text=f"{i+1}\n{self.names[i]}")

root = tk.Tk()
app = Library(root)
root.mainloop()

```

