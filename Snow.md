class ParkingLotSystem:

    def __init__(self):
        self.vehicles = {}

    def add_vehicle(self, vehicle_no: str, owner: str, slot: str) -> dict:
        if vehicle_no in self.vehicles:
            raise ValueError("Vehicle already exists")

        self.vehicles[vehicle_no] = {
            "owner": owner,
            "slot": slot,
            "status": "Parked"
        }

        return self.vehicles

    def update_slot(self, vehicle_no: str, new_slot: str) -> dict:
        if vehicle_no not in self.vehicles:
            raise KeyError("Vehicle not found")

        self.vehicles[vehicle_no]["slot"] = new_slot

        return self.vehicles

    def get_vehicle_details(self, vehicle_no: str) -> dict:
        if vehicle_no not in self.vehicles:
            raise KeyError("Vehicle not found")

        return self.vehicles[vehicle_no]

    def vehicles_by_zone(self, zone_prefix: str) -> list:
        result = []

        for vehicle_no in self.vehicles:
            if self.vehicles[vehicle_no]["slot"].startswith(zone_prefix):
                result.append(vehicle_no)

        return result
