from flask import Flask, request, jsonify
from flask_pymongo import PyMongo

app = Flask(__name__)
app.config['MONGO_URI'] = 'mongodb://localhost:27017/salary_management'
mongo = PyMongo(app)

@app.route('/calculate_salary', methods=['POST'])
def calculate_salary():
    try:
        data = request.get_json()

        # Perform the salary calculation and MongoDB insertion here
        # Assuming MongoDB collection named 'employees'
        db = mongo.db.employees

        # Assuming 'employee_id' is unique
        existing_employee = db.find_one({'employee_id': data['employee_id']})

        if existing_employee:
            return jsonify({'message': 'Employee ID already exists.'}), 400

        # Perform the salary calculation logic here
        # ...

        # Insert into MongoDB
        db.insert_one(data)

        return jsonify({'message': 'Salary details stored successfully.'}), 201

    except Exception as e:
        return jsonify({'error': str(e)}), 500

@app.route('/employees', methods=['GET'])
def get_employees():
    try:
        # Retrieve all employees from MongoDB
        db = mongo.db.employees
        employees = list(db.find({}, {'_id': 0}))

        return jsonify(employees), 200

    except Exception as e:
        return jsonify({'error': str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True)
