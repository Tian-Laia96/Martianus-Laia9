# untuk membuat table
from tabulate import tabulate

# square root, untuk menghitung euclidean distance
from math import sqrt


class Membership:
    # key adalah username, value adalah tier membership
    data = {
        "Sumbul": "Platinum",
        "Ana": "Gold",
        "Cahya": "Platinum"
    }
    
    def __init__(self):
        pass

    # implement fitur pertama, show tier
    def show_benefit(self):
        # init headers
        headers = ["Membership", "Discount", "Another Benefit"]
        
        # init data
        table = [
            ["Silver", "8%", "Voucher Makanan"],
            ["Gold", "10%", "Benefit Silver + Voucher Ojek Online"],
            ["Platinum", "15%", "Benefit Silver + Gold + Cashback max. 30%"]
        ]
        
        # show tabulate
        print("PacCommerce Tier Membership")
        print("")
        print(tabulate(table, headers, tablefmt = "github"))

    # implement fitur kedua, show requirements
    def show_requirements(self):
        # init headers
        headers = ["Membership", "Monthly Expense (Juta)", "Monthly Income (Juta)"]
        
        # init data
        table = [
            ["Silver", 5, 7],
            ["Gold", 6, 10],
            ["Platinum", 8, 15]
        ]
        
        # show tabulate
        print("PacCommerce Requirements Membership")
        print("")
        print(tabulate(table, headers, tablefmt = "pretty"))

    # implement fitur ketiga, predict membership berdasarkan input
    def predict_membership(self, username: str, monthly_expense: float, monthly_income: float):
        try:
            # buat empty list untuk menampung hasil result
            tmp_result = []
            
            # list parameter membership
            parameter_membership = [[8, 15], [6, 10], [5, 7]]
            
            # create defense mechanism
            # 1. cek expense < income
            if monthly_expense <= monthly_income:
                # iterasi data parameter
                for idx in range(len(parameter_membership)):
                    euclidean = round(sqrt((monthly_expense - parameter_membership[idx][0])**2 + \
                                     (monthly_income - parameter_membership[idx][1])**2), 2)

                    # store result to list
                    tmp_result.append(euclidean)

                dict_result = {
                    "Platinum": tmp_result[0],
                    "Gold": tmp_result[1],
                    "Silver": tmp_result[2]
                }

                print(f"Hasil perhitungan Euclidean Distance dari user {username} adalah {dict_result}")

                # get minimum value from list
                get_min_value = min(tmp_result)

                for keys, values in dict_result.items():
                    if get_min_value == values:
                        # insert new predict data to existing data
                        self.data[username] = keys
                        
                        return keys
                
            else:
                raise Exception("Input Expense harus lebih kecil dari input income")

        except Exception as e:
            raise Exception(e)

    # implement fitur keempat, calculate price dengan input username dan list_of_harga
    def calculate_price(self, username: str, list_of_price: list):
        # get membership based on username
        get_membership = self.data.get(username)
        sum_price = sum(list_of_price)

        if get_membership == "Platinum":
            total_price = sum_price - (sum_price * 0.15)

            return total_price

        elif get_membership == "Gold":
            total_price = sum_price - (sum_price * 0.10)

            return total_price

        elif get_membership == "Silver":
            total_price = sum_price - (sum_price * 0.08)

            return total_price

        else:
            raise Exception("Membership tidak valid")

obj_1 = Membership()

print(obj_1.data)

# predict membership
obj_1.predict_membership(username = "Shandy",
                         monthly_expense = 9,
                         monthly_income = 12)

# updated data
print(obj_1.data)
