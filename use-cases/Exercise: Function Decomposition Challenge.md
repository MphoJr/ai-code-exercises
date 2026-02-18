Step 1: Identify Distinct Responsibilities
Looking at the function, it’s doing a lot:
- Required field checks (registration vs. profile update).
- Username validation (length, characters, uniqueness).
- Password validation (length, complexity, confirmation).
- Email validation (format, uniqueness).
- Date of birth validation (valid date, age range).
- Address validation (required fields, ZIP/postal code formats).
- Phone validation (basic regex).
- Custom field validations (user-provided rules).

📝 Step 2: Decomposition Plan
Break the function into smaller helpers, each with a clear purpose:
- validateRequiredFields(userData, isRegistration)
- validateUsername(userData, options)
- validatePassword(userData)
- validateEmail(userData, options)
- validateDateOfBirth(userData)
- validateAddress(userData)
- validatePhone(userData)
- applyCustomValidations(userData, options)
The main function becomes an orchestrator that calls each helper and aggregates errors.

🛠 Step 3: Extract Helper Functions (Examples)
function validateUsername(userData, options) {
  const errors = [];
  const username = userData.username;
  if (!username) return errors;

  if (username.length < 3) {
    errors.push('Username must be at least 3 characters long');
  } else if (username.length > 20) {
    errors.push('Username must be at most 20 characters long');
  } else if (!/^[a-zA-Z0-9_]+$/.test(username)) {
    errors.push('Username can only contain letters, numbers, and underscores');
  } else if (options.checkExisting && options.checkExisting.usernameExists(username)) {
    errors.push('Username is already taken');
  }

  return errors;
}

function validatePassword(userData) {
  const errors = [];
  const password = userData.password;
  if (!password) return errors;

  if (password.length < 8) errors.push('Password must be at least 8 characters long');
  if (!/[A-Z]/.test(password)) errors.push('Password must contain at least one uppercase letter');
  if (!/[a-z]/.test(password)) errors.push('Password must contain at least one lowercase letter');
  if (!/[0-9]/.test(password)) errors.push('Password must contain at least one number');
  if (!/[^A-Za-z0-9]/.test(password)) errors.push('Password must contain at least one special character');

  if (userData.confirmPassword !== password) {
    errors.push('Password and confirmation do not match');
  }

  return errors;
}



🧩 Step 4: Refactor Main Function
function validateUserData(userData, options = {}) {
  const errors = [];
  const isRegistration = options.isRegistration || false;

  if (isRegistration) {
    errors.push(...validateRequiredFields(userData, true));
    errors.push(...validateUsername(userData, options));
    errors.push(...validatePassword(userData));
  } else {
    errors.push(...validateRequiredFields(userData, false));
  }

  errors.push(...validateEmail(userData, options));
  errors.push(...validateDateOfBirth(userData));
  errors.push(...validateAddress(userData));
  errors.push(...validatePhone(userData));

  if (options.customValidations) {
    errors.push(...applyCustomValidations(userData, options));
  }

  return errors;
}



✅ Step 5: Verify Behavior
- Run existing tests (or create new ones) to ensure refactored code produces identical results.
- Test edge cases (missing fields, invalid formats, extreme ages, etc.).

📈 Benefits Gained
- Readability: Each helper function has a clear, single responsibility.
- Maintainability: Easier to update rules (e.g., password complexity) without touching unrelated logic.
- Testability: Each helper can be unit tested independently.
- Extensibility: Adding new validations (e.g., new country ZIP formats) is straightforward.
