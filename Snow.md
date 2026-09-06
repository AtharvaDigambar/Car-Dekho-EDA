class CafeteriaOrderSystem:
    def __init__(self):
        self.data = {}

    def add_order(self, employee_id, name, meal_type, quantity):
        if employee_id in self.data:
            raise ValueError("Order already exists")
        self.data[employee_id] = {
            "name": name,
            "meal_type": meal_type,
            "quantity": quantity,
            "status": "Confirmed"
        }
        return self.data

    def update_quantity(self, employee_id, new_quantity):
        if employee_id not in self.data:
            raise KeyError("Order not found")
        self.data[employee_id]["quantity"] = new_quantity
        return self.data

    def get_order_details(self, employee_id):
        if employee_id not in self.data:
            raise KeyError("Order not found")
        return self.data[employee_id]

    def get_bulk_orders(self, minimum_quantity):
        return [eid for eid, info in self.data.items()
                if info["quantity"] >= minimum_quantity]
