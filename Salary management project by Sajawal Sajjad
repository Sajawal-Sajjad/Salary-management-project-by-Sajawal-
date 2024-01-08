# Submitted by   : Sajawal Sajjad
# Rollno         : F21BSEEN1E02037
# Semester       : 5th
# Section        : E1
# Project        : Salary Management System
# Submitted To   : Sir Nauman

import tkinter as tk
from tkinter import ttk, messagebox
import requests

class SalaryManagementSystem:
    def __init__(self, root):
        self.root = root
        self.root.title("Salary Management System")

        # Entry fields for adding new employee
        self.employee_id_label = ttk.Label(root, text="Employee ID:")
        self.employee_id_label.grid(row=0, column=0, sticky='E', pady=5)
        self.employee_id_entry = ttk.Entry(root)
        self.employee_id_entry.grid(row=0, column=1, pady=5)

        self.name_label = ttk.Label(root, text="Employee Name:")
        self.name_label.grid(row=1, column=0, sticky='E', pady=5)
        self.name_entry = ttk.Entry(root)
        self.name_entry.grid(row=1, column=1, pady=5)

        self.position_label = ttk.Label(root, text="Position:")
        self.position_label.grid(row=2, column=0, sticky='E', pady=5)
        self.position_var = tk.StringVar()
        self.position_var.set("Software Developer")  # Default position
        self.position_dropdown = ttk.Combobox(root, textvariable=self.position_var, values=["Software Developer", "Manager", "Intern"])
        self.position_dropdown.grid(row=2, column=1, pady=5)

        self.hours_worked_label = ttk.Label(root, text="Hours Worked:")
        self.hours_worked_label.grid(row=3, column=0, sticky='E', pady=5)
        self.hours_worked_entry = ttk.Entry(root)
        self.hours_worked_entry.grid(row=3, column=1, pady=5)

        self.bill_type_label = ttk.Label(root, text="Bill Type:")
        self.bill_type_label.grid(row=4, column=0, sticky='E', pady=5)
        self.bill_type_entry = ttk.Entry(root)
        self.bill_type_entry.grid(row=4, column=1, pady=5)

        self.bill_amount_label = ttk.Label(root, text="Bill Amount:")
        self.bill_amount_label.grid(row=5, column=0, sticky='E', pady=5)
        self.bill_amount_entry = ttk.Entry(root)
        self.bill_amount_entry.grid(row=5, column=1, pady=5)

        self.salary_label = ttk.Label(root, text="Salary:")
        self.salary_label.grid(row=6, column=0, sticky='E', pady=5)
        self.salary_entry = ttk.Entry(root)  # Editable field
        self.salary_entry.grid(row=6, column=1, pady=5)

        self.net_salary_label = ttk.Label(root, text="Net Salary:")
        self.net_salary_label.grid(row=7, column=0, sticky='E', pady=5)
        self.net_salary_entry = ttk.Entry(root)  # Editable field
        self.net_salary_entry.grid(row=7, column=1, pady=5)

        # Button to calculate salary and add employee
        self.calculate_button = ttk.Button(root, text="Calculate Salary", command=self.calculate_and_add_employee)
        self.calculate_button.grid(row=8, column=0, columnspan=2, pady=10)

        # Button to refresh the employee data in Treeview
        self.refresh_button = ttk.Button(root, text='Refresh', command=self.refresh_data)
        self.refresh_button.grid(row=9, column=0, columnspan=2, pady=10)

        # Treeview to display employee data
        self.tree = ttk.Treeview(self.root, columns=('Employee ID', 'Name', 'Position', 'Hours Worked', 'Salary', 'Net Salary'), show='headings')
        self.tree.heading('Employee ID', text='Employee ID')
        self.tree.heading('Name', text='Name')
        self.tree.heading('Position', text='Position')
        self.tree.heading('Hours Worked', text='Hours Worked')
        self.tree.heading('Salary', text='Salary')
        self.tree.heading('Net Salary', text='Net Salary')
        self.tree.grid(row=10, column=0, columnspan=6, pady=10)

    def calculate_and_add_employee(self):
        try:
            employee_id = int(self.employee_id_entry.get())
            employee_name = self.name_entry.get()
            position = self.position_var.get()
            hours_worked = float(self.hours_worked_entry.get())

            # Define salary configuration templates based on position
            salary_templates = {
                "Software Developer": {"hourly_rate": 30, "bonus": 500},
                "Manager": {"hourly_rate": 40, "bonus": 1000},
                "Intern": {"hourly_rate": 15, "bonus": 100}
            }

            # Get the salary template for the selected position
            template = salary_templates.get(position)

            if template:
                hourly_rate = template["hourly_rate"]
                bonus = template["bonus"]
                salary = hours_worked * hourly_rate + bonus

                # Get bills information
                bill_type = self.bill_type_entry.get()
                bill_amount = float(self.bill_amount_entry.get()) if self.bill_amount_entry.get() else 0

                # Calculate net salary by deducting bills
                net_salary = salary - bill_amount

                # Update the Salary and Net Salary entry fields
                self.salary_entry.delete(0, tk.END)
                self.salary_entry.insert(0, salary)

                self.net_salary_entry.delete(0, tk.END)
                self.net_salary_entry.insert(0, net_salary)

                # Make API call to store employee details
                api_url = 'http://127.0.0.1:5000/calculate_salary'
                payload = {
                    'employee_id': employee_id,
                    'name': employee_name,
                    'position': position,
                    'hours_worked': hours_worked,
                    'bill_type': bill_type,
                    'bill_amount': bill_amount,
                    'salary': salary,
                    'net_salary': net_salary
                }
                response = requests.post(api_url, json=payload)

                if response.status_code == 201:
                    messagebox.showinfo("Salary Details", f"Employee ID: {employee_id}\n"
                                                          f"Name: {employee_name}\n"
                                                          f"Position: {position}\n"
                                                          f"Hours Worked: {hours_worked}\n"
                                                          f"Hourly Rate: {hourly_rate}\n"
                                                          f"Bonus: {bonus}\n"
                                                          f"Bills - {bill_type}: {bill_amount}\n"
                                                          f"Salary: {salary}\n"
                                                          f"Net Salary: {net_salary}")
                else:
                    messagebox.showerror("Error", "Failed to store salary details.")
            else:
                messagebox.showerror("Error", "Invalid position selected.")
        except ValueError:
            messagebox.showerror("Error", "Please enter valid numeric values for Employee ID, Hours Worked, and Bill Amount.")

    def refresh_data(self):
        # Clear existing data in the Treeview
        for item in self.tree.get_children():
            self.tree.delete(item)

        # Retrieve data from the API and populate the new Treeview
        response = requests.get('http://127.0.0.1:5000/employees')
        employees = response.json()

        for employee in employees:
            self.tree.insert('', 'end', values=(
                employee['employee_id'],
                employee['name'],
                employee['position'],
                employee['hours_worked'],
                employee['salary'],
                employee['net_salary']
            ))

if __name__ == '__main__':
    root = tk.Tk()
    app = SalaryManagementSystem(root)
    root.mainloop()
