<template>
  <div>
    <h2>Employee with Highest Sales</h2>
    <p>Name: {{ topEmployee.name }}</p>
    <p>Email: {{ topEmployee.email }}</p>
    <p>Email: {{ calculateTotalSales(topEmployee.sales) }}</p>

    <h2>All Employees</h2>
    <div>
      <button @click="sortEmployees('asc')">Sort Ascending</button>
      <button @click="sortEmployees('desc')">Sort Descending</button>
    </div>
    <ul>
      <li v-for="employee in sortedEmployees" :key="employee.email">
        <h3>{{ employee.name }}</h3>
        <p>Email: {{ employee.email }}</p>
        <p>Total Sales: {{ calculateTotalSales(employee.sales) }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: [
        {
          name: "John",
          email: "john3@example.com",
          sales: [
            { customer: "The Blue Rabbit Company", order_total: 7444 },
            { customer: "Black Melon", order_total: 1445 },
            { customer: "Foggy Toaster", order_total: 700 },
          ],
        },
        {
          name: "Jane",
          email: "jane8@example.com",
          sales: [
            { customer: "The Grey Apple Company", order_total: 203 },
            { customer: "Yellow Cake", order_total: 8730 },
            { customer: "The Piping Bull Company", order_total: 3337 },
            { customer: "The Cloudy Dog Company", order_total: 5310 },
          ],
        },
        {
          name: "Dave",
          email: "dave1@example.com",
          sales: [
            { customer: "The Acute Toaster Company", order_total: 1091 },
            { customer: "Green Mobile", order_total: 2370 },
          ],
        },
      ],
      sortDirection: "asc",
    };
  },
  computed: {
    topEmployee() {
      return this.employees.reduce((prevEmployee, currentEmployee) => {
        const prevTotalSales = this.calculateTotalSales(prevEmployee.sales);
        const currentTotalSales = this.calculateTotalSales(
          currentEmployee.sales
        );
        return prevTotalSales > currentTotalSales
          ? prevEmployee
          : currentEmployee;
      });
    },
    sortedEmployees() {
      const sorted = [...this.employees];
      sorted.sort((a, b) => {
        const aTotalSales = this.calculateTotalSales(a.sales);
        const bTotalSales = this.calculateTotalSales(b.sales);
        if (this.sortDirection === "asc") {
          return aTotalSales - bTotalSales;
        } else {
          return bTotalSales - aTotalSales;
        }
      });
      return sorted;
    },
  },
  methods: {
    calculateTotalSales(sales) {
      return sales.reduce((total, sale) => total + sale.order_total, 0);
    },
    sortEmployees(direction) {
      this.sortDirection = direction;
    },
  },
};
</script>
