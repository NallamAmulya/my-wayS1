# my-wayS1
import React, { useState, useEffect } from "react";
import { BrowserRouter as Router, Switch, Route, NavLink } from "react-router-dom";
import "./App.css";

function App() {
  return (
    <Router>
      <nav>
        <ul>
          <li>
            <NavLink exact activeClassName="active" to="/">Home</NavLink>
          </li>
          <li>
            <NavLink activeClassName="active" to="/foods">Foods</NavLink>
          </li>
        </ul>
      </nav>

      <Switch>
        <Route exact path="/">
          <FoodForm />
        </Route>
        <Route path="/foods">
          <FoodList />
        </Route>
      </Switch>
    </Router>
  );
}

function FoodForm() {
  const [foodName, setFoodName] = useState("");
  const [foodType, setFoodType] = useState("Delicious Food");
  const [maxDeliveryTime, setMaxDeliveryTime] = useState("");
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const newFood = {
      name: foodName,
      type: foodType,
      maxDeliveryTime: maxDeliveryTime,
    };
    const existingFoods = JSON.parse(localStorage.getItem("foods")) || [];
    localStorage.setItem("foods", JSON.stringify([...existingFoods, newFood]));
    setFoodName("");
    setFoodType("Delicious Food");
    setMaxDeliveryTime("");
    alert("Food item added!");
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Food Name:
        <input type="text" value={foodName} onChange={(e) => setFoodName(e.target.value)} required />
      </label>
      <br />
      <label>
        Food Type:
        <select value={foodType} onChange={(e) => setFoodType(e.target.value)} required>
          <option value="Delicious Food">Delicious Food</option>
          <option value="Nutritious Food">Nutritious Food</option>
          <option value="Fast Food">Fast Food</option>
          <option value="Beverages">Beverages</option>
          <option value="Desserts">Desserts</option>
        </select>
      </label>
      <br />
      <label>
        Max Delivery Time (minutes):
        <input type="number" value={maxDeliveryTime} onChange={(e) => setMaxDeliveryTime(e.target.value)} required />
      </label>
      <br />
      <button type="submit">Add Food Item</button>
    </form>
  );
}

function FoodList() {
  const [foods, setFoods] = useState([]);
  const [filterType, setFilterType] = useState("");
  const [filterDeliveryTime, setFilterDeliveryTime] = useState("");

  useEffect(() => {
    const savedFoods = JSON.parse(localStorage.getItem("foods")) || [];
    setFoods(savedFoods);
  }, []);

  const handleFilterTypeChange = (event) => {
    setFilterType(event.target.value);
  };

  const handleFilterDeliveryTimeChange = (event) => {
    setFilterDeliveryTime(event.target.value);
  };

  const filteredFoods = foods.filter(
    (food) =>
      (!filterType || food.type === filterType) &&
      (!filterDeliveryTime || food.maxDeliveryTime <= filterDeliveryTime)
  );
