class ParcelTrackingSystem:
    def __init__(self):
        self.data = {}

    def add_parcel(self, tracking_id, customer_name, destination, weight):
        if tracking_id in self.data:
            raise ValueError("Parcel already exists")
        self.data[tracking_id] = {
            "customer_name": customer_name,
            "destination": destination,
            "weight": weight,
            "status": "In Transit"
        }
        return self.data

    def update_weight(self, tracking_id, new_weight):
        if tracking_id not in self.data:
            raise KeyError("Parcel not found")
        self.data[tracking_id]["weight"] = new_weight
        return self.data

    def get_parcel_details(self, tracking_id):
        if tracking_id not in self.data:
            raise KeyError("Parcel not found")
        return self.data[tracking_id]

    def get_heavy_parcels(self, minimum_weight):
        return [tid for tid, info in self.data.items()
                if info["weight"] >= minimum_weight]
