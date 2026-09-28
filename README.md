// ==========================================
// 1. DATA STORAGE (Arrays & State)
// ==========================================
const expenses = [];
const BUDGET_LIMIT = 500; // Define a benchmark threshold for budget evaluation

// ==========================================
// 2. DOM ELEMENT SELECTION
// ==========================================
const expenseForm = document.getElementById('expense-form');
const nameInput = document.getElementById('expense-name');
const amountInput = document.getElementById('expense-amount');
const categoryInput = document.getElementById('expense-category');

const tableBody = document.getElementById('expense-table-body');
const totalExpenseDisplay = document.getElementById('total-expense-display');
const totalCountDisplay = document.getElementById('total-count-display');
const budgetAlert = document.getElementById('budget-alert');

// ==========================================
// 3. EVENT LISTENERS
// ==========================================
expenseForm.addEventListener('submit', function (event) {
  event.preventDefault(); // Prevent page refresh on submission

  // Extract form input values
  const nameValue = nameInput.value.trim();
  const amountValue = parseFloat(amountInput.value);
  const categoryValue = categoryInput.value;

  // Validate inputs
  if (nameValue !== '' && !isNaN(amountValue) && amountValue > 0) {
    // Add new expense object to array
    const newExpense = {
      id: Date.now(),
      name: nameValue,
      amount: amountValue,
      category: categoryValue
    };

    expenses.push(newExpense);

    // Update the UI
    renderExpenses();
    updateDashboard();

    // Reset form fields
    expenseForm.reset();
  }
});

// ==========================================
// 4. LOOPS & DOM MANIPULATION
// ==========================================
function renderExpenses() {
  // Clear existing table content to prevent duplicate displays
  tableBody.innerHTML = '';

  // Loop through the array of expenses using a for loop (or forEach)
  for (let i = 0; i < expenses.length; i++) {
    const expense = expenses[i];

    // Create dynamic HTML table row
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${expense.name}</td>
      <td>${expense.category}</td>
      <td>$${expense.amount.toFixed(2)}</td>
    `;

    // Append created row to table body
    tableBody.appendChild(row);
  }
}

// ==========================================
// 5. DECISION MAKING & CALCULATIONS (Conditionals)
// ==========================================
function updateDashboard() {
  let total = 0;

  // Use a loop to calculate total expenses
  for (let i = 0; i < expenses.length; i++) {
    total += expenses[i].amount;
  }

  // Update total DOM elements
  totalExpenseDisplay.textContent = `$${total.toFixed(2)}`;
  totalCountDisplay.textContent = expenses.length;

  // Evaluate budgeting status using conditional statements (If/Else)
  budgetAlert.classList.remove('hidden', 'success', 'warning', 'danger');

  if (total === 0) {
    budgetAlert.classList.add('hidden');
  } else if (total < BUDGET_LIMIT * 0.75) {
    budgetAlert.textContent = "Great job! Your spending is well within budget limits.";
    budgetAlert.classList.add('success');
  } else if (total >= BUDGET_LIMIT * 0.75 && total <= BUDGET_LIMIT) {
    budgetAlert.textContent = "Warning: You are approaching your target budget threshold!";
    budgetAlert.classList.add('warning');
  } else {
    budgetAlert.textContent = "Budget Alert: You have exceeded your targeted budget limit!";
    budgetAlert.classList.add('danger');
  }
}
