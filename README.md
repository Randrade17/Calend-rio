Calendário Funcional em Python

Este script cria uma interface gráfica de calendário utilizando o módulo Tkinter.

Funcionalidades principais:
- Exibe o calendário do mês e ano atual.
- Permite navegar entre meses usando botões.
- Mostra os dias organizados em colunas (segunda a domingo).

Requisitos:
- Python 3 instalado.

Como usar:
1. Execute o script.
2. A janela será aberta com o calendário.
3. Use os botões "Mês Anterior" e "Próximo Mês" para navegar.

Autor: Rafael Figueiredo de Andrade
Data: 11/06/2025




import tkinter as tk
from tkinter import ttk
import calendar
from datetime import datetime

class CalendarApp:
    def __init__(self, master):
        self.master = master
        self.master.title("Calendário")
        
        # Data atual
        now = datetime.now()
        self.year = now.year
        self.month = now.month

        # Cabeçalho com navegação
        self.header = ttk.Frame(master)
        self.header.pack(pady=10)

        self.prev_button = ttk.Button(self.header, text="<< Mês Anterior", command=self.prev_month)
        self.prev_button.grid(row=0, column=0)

        self.next_button = ttk.Button(self.header, text="Próximo Mês >>", command=self.next_month)
        self.next_button.grid(row=0, column=2)

        self.title_label = ttk.Label(self.header, text="", font=("Helvetica", 16))
        self.title_label.grid(row=0, column=1, padx=20)

        # Área do calendário
        self.calendar_frame = ttk.Frame(master)
        self.calendar_frame.pack()

        self.show_calendar()

    def show_calendar(self):
        # Limpa o frame
        for widget in self.calendar_frame.winfo_children():
            widget.destroy()

        # Atualiza título
        self.title_label.config(text=f"{calendar.month_name[self.month]} {self.year}")

        # Cabeçalhos dos dias
        days = ["Seg", "Ter", "Qua", "Qui", "Sex", "Sáb", "Dom"]
        for idx, day in enumerate(days):
            ttk.Label(self.calendar_frame, text=day, borderwidth=1, relief="solid", width=5).grid(row=0, column=idx)

        # Cria o calendário
        cal = calendar.Calendar(firstweekday=0)  # Começa na segunda-feira
        month_days = cal.monthdayscalendar(self.year, self.month)

        for row_idx, week in enumerate(month_days, start=1):
            for col_idx, day in enumerate(week):
                if day == 0:
                    cell = ""
                else:
                    cell = str(day)
                ttk.Label(
                    self.calendar_frame,
                    text=cell,
                    borderwidth=1,
                    relief="solid",
                    width=5,
                    anchor="center"
                ).grid(row=row_idx, column=col_idx)

    def prev_month(self):
        # Vai para o mês anterior
        if self.month == 1:
            self.month = 12
            self.year -= 1
        else:
            self.month -= 1
        self.show_calendar()

    def next_month(self):
        # Vai para o próximo mês
        if self.month == 12:
            self.month = 1
            self.year += 1
        else:
            self.month += 1
        self.show_calendar()

if __name__ == "__main__":
    root = tk.Tk()
    app = CalendarApp(root)
    root.mainloop()
