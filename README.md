# RTP Techie Repair - Complete Service Management Platform

## 📱 Overview

**RTP Techie Repair** is a comprehensive **full-stack web application** for managing appliance repair services. It connects customers, technicians, and product sellers in a single integrated platform, enabling efficient service booking, issue tracking, and repair management.

Perfect for:
- **Service Management**: Complete appliance repair booking system
- **Multi-Role Platform**: Customers, technicians, and sellers
- **Real-Time Tracking**: Issue assignment and tracking
- **User Authentication**: Secure login with role-based access
- **Business Management**: Dashboard for each user type
- **Modern Web Stack**: Node.js, Express, MongoDB, EJS

---

## 🎯 Project Purpose

RTP Techie Repair provides:
1. **Customer Platform** - Book repair services, view technicians, track issues
2. **Technician Platform** - Manage assigned repairs, view customer issues
3. **Product Seller Platform** - Manage replacement parts and products
4. **Unified Database** - Centralized issue and user management

---

## 📁 Project Structure

```
RTP-Techie-Repair/
├── Backend/
│   ├── index.js                 # Main server and routes
│   └── config.js                # Database configuration & schemas
│
├── interface/
│   ├── login.ejs                # Login page (all users)
│   ├── csignup.ejs              # Customer signup page
│   ├── tsignup.ejs              # Technician signup page
│   ├── psignup.ejs              # Product seller signup page
│   ├── cs.ejs                   # Customer dashboard
│   ├── technician.ejs           # Technician dashboard
│   └── ps.ejs                   # Product seller dashboard
│
├── .git/                        # Version control
└── README.md                    # This file
```

---

## 🏗 Technology Stack

### **Backend**
- **Node.js** - JavaScript runtime
- **Express.js** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB ODM
- **bcrypt** - Password hashing
- **express-session** - Session management

### **Frontend**
- **EJS** - Templating engine
- **Bootstrap 4/5** - CSS framework
- **HTML5** - Markup
- **CSS3** - Styling
- **JavaScript (Vanilla)** - Client-side logic

### **Development Tools**
- **npm** - Package manager
- **Git** - Version control

---

## 📊 Database Models

### **User Schema** (Loginschema)
```javascript
{
  name: String (required),
  email: String (required, unique),
  password: String (required, hashed),
  phone: String (required),
  role: String (required) // "customer", "technician", "Product Seller"
  
  // For technicians only:
  rating: Number (default: 0-5),
  specialization: String (default: "General")
}
```

### **Issue Schema**
```javascript
{
  name: String (required),              // Customer name
  contactNumber: String (required),     // Customer phone
  email: String (required),             // Customer email
  applianceType: String (required),     // e.g., "Fridge", "AC", "Washing Machine"
  brand: String (required),             // Appliance brand
  issue: String (required),             // Problem description
  date: Date (required),                // Appointment date
  contactMethod: String (required),     // "Phone", "Email", "WhatsApp"
  technician: ObjectId (required, ref: User)  // Assigned technician
}
```

---

## 🚀 Key Features

### **1. User Authentication**
- Secure login with email and password
- Password hashing with bcrypt (10 salt rounds)
- Session-based authentication
- Role-based access control

### **2. Customer Features**
- ✅ Account creation with name, email, phone
- ✅ Browse available technicians with ratings and specializations
- ✅ Submit repair requests with:
  - Appliance type and brand
  - Problem description
  - Preferred appointment date
  - Contact method preference
  - Technician selection
- ✅ View submitted issues status
- ✅ Dashboard with all active issues
- ✅ Secure logout

### **3. Technician Features**
- ✅ Account creation with specialization
- ✅ Auto-assigned rating (1-5 stars)
- ✅ View all assigned issues
- ✅ See customer details for each issue
- ✅ Track repair request status
- ✅ Delete completed issues
- ✅ Dashboard with issue management

### **4. Product Seller Features**
- ✅ Account creation and login
- ✅ View all orders/issues in system
- ✅ Dashboard with order management
- ✅ Access to customer information
- ✅ Inventory tracking capability

### **5. Admin/System Features**
- ✅ User account management
- ✅ Issue tracking and assignment
- ✅ Database persistence
- ✅ Error handling

---

## 🔗 REST API Routes

### **Authentication Routes**

**GET /login** - Display login page
```html
Response: login.ejs with login form
```

**POST /login** - Submit login credentials
```javascript
Request: { email, password }
Response: Redirect to role-specific dashboard
Validations: Email exists, password matches (bcrypt)
```

**GET /logout** - Destroy session and logout
```javascript
Response: Redirect to login page
```

---

### **Signup Routes**

**Customer Signup**:
- **GET /csignup** - Display customer signup form
- **POST /csignup** - Create customer account
  - Fields: name, email, password, phone
  - Role automatically set to "customer"
  - Validation: Email uniqueness, password hashing

**Technician Signup**:
- **GET /tsignup** - Display technician signup form
- **POST /tsignup** - Create technician account
  - Fields: name, email, password, phone, specialization
  - Role automatically set to "technician"
  - Rating auto-assigned (1-5)
  - Validation: Email uniqueness, password hashing

**Product Seller Signup**:
- **GET /psignup** - Display seller signup form
- **POST /psignup** - Create seller account
  - Fields: name, email, password, phone
  - Role automatically set to "Product Seller"
  - Validation: Email uniqueness, password hashing

---

### **Dashboard Routes**

**Customer Dashboard**:
- **GET /cs** - Display customer dashboard
  - Requires session with customer role
  - Shows list of available technicians
  - Displays issue submission form
  - Shows submitted issues

**Technician Dashboard**:
- **GET /technician** - Display technician dashboard
  - Requires session with technician role
  - Shows all issues assigned to this technician
  - Displays customer details for each issue
  - Shows issue submission date and contact method

**Product Seller Dashboard**:
- **GET /ps** - Display seller dashboard
  - Requires session with Product Seller role
  - Shows all orders/issues in system
  - Access to customer information

---

### **Issue Management Routes**

**Submit Repair Request**:
- **POST /submitRequest** - Create new repair issue
  ```javascript
  Request: {
    name,
    contactNumber,
    email,
    applianceType,
    brand,
    issue,
    date,
    contactMethod,
    technician  // Technician ID
  }
  ```
  - Validations: All fields required
  - Creates Issue document linked to technician
  - Response: Success message on customer dashboard

**Delete Issue**:
- **POST /delete/:id** - Remove issue by ID
  - Requires technician authentication
  - Soft/hard delete from database
  - Response: Redirect to technician dashboard

---

## 🎨 User Interfaces (EJS Templates)

### **1. login.ejs** (443 lines)
**Purpose**: Universal login page for all users

**Features**:
- Email input field
- Password input field
- Submit button
- Signup links for each role
- Message display (errors/success)
- Responsive design
- Custom CSS styling

**Styling**:
- Clean form layout
- Input validation styling
- Hover effects
- Centered layout

---

### **2. csignup.ejs** (172 lines)
**Purpose**: Customer signup form

**Fields**:
- Name (required)
- Email (required, unique)
- Password (required)
- Phone (required)
- Submit button
- Back/Close option

**Features**:
- Form validation
- Error messaging
- Responsive layout
- Custom styling

---

### **3. tsignup.ejs**
**Purpose**: Technician signup form

**Fields**:
- Name (required)
- Email (required, unique)
- Password (required)
- Phone (required)
- Specialization (optional)
- Submit button

**Specialization Options**:
- General
- Electronics
- Appliances
- Air Conditioning
- Custom specialization

---

### **4. psignup.ejs**
**Purpose**: Product seller signup form

**Fields**:
- Name (required)
- Email (required, unique)
- Password (required)
- Phone (required)
- Submit button

---

### **5. cs.ejs** (236 lines)
**Purpose**: Customer dashboard

**Sections**:
1. **Navigation Bar**
   - Dashboard home link
   - About, Contact links
   - Logout button

2. **Welcome Section**
   - Greeting with user name
   - Quick action buttons

3. **Technician List**
   - Cards showing available technicians
   - Display: Name, rating, specialization, phone

4. **Issue Submission Form**
   - Appliance type dropdown
   - Brand input
   - Issue description
   - Preferred date selector
   - Contact method (Phone/Email/WhatsApp)
   - Technician selection dropdown
   - Submit button

5. **Submitted Issues Table**
   - Shows all customer's submitted issues
   - Displays: Appliance, Brand, Issue, Status
   - Assigned technician information
   - Issue tracking

**Styling**:
- Bootstrap 4 framework
- Responsive grid layout
- Cards for technician display
- Tables for issue tracking

---

### **6. technician.ejs** (258 lines)
**Purpose**: Technician dashboard

**Sections**:
1. **Navigation Bar**
   - Dashboard title
   - Home, About, Contact links
   - Logout button
   - Technician name display

2. **Profile Section**
   - Display technician info
   - Rating display
   - Specialization
   - Contact information

3. **Assigned Issues Table**
   - Shows all issues assigned to technician
   - Columns: Customer name, phone, email
   - Appliance info (type, brand)
   - Problem description
   - Appointment date
   - Contact method
   - Action button: Delete/Complete

4. **Issue Management Form**
   - Update issue status (optional)
   - Contact customer buttons

**Styling**:
- Bootstrap 5 framework
- Responsive table layout
- Color-coded status indicators
- Action buttons with icons

---

### **7. ps.ejs** (297 lines)
**Purpose**: Product seller dashboard

**Sections**:
1. **Navigation Bar**
   - Dashboard title
   - Navigation links
   - Logout button

2. **Orders/Issues Table**
   - Shows all orders in system
   - Customer information
   - Appliance details
   - Issue description
   - Status tracking

3. **Product Management** (optional)
   - Manage replacement parts
   - Inventory tracking

**Styling**:
- Bootstrap 5 framework
- Professional table layout
- Color-coded information

---

## 🔐 Security Features

### **Password Security**
- bcrypt hashing with 10 salt rounds
- Passwords never stored in plaintext
- Comparison using bcrypt.compare()

### **Session Management**
- Express-session for authentication
- Session data stored server-side
- Secure cookie settings
- Automatic session destruction on logout

### **Access Control**
- Role-based authentication
- Route protection (redirect if unauthorized)
- Role validation on each protected route
- Session user verification

### **Data Validation**
- Email uniqueness check before signup
- All forms validate required fields
- Error messages for invalid inputs
- Database constraint checks

---

## 📋 User Workflows

### **Customer Workflow**
```
1. Register → Sign up with email, name, phone, password
2. Login → Enter credentials, authenticate with bcrypt
3. View Technicians → Browse available technicians with ratings
4. Select Technician → Choose based on specialization/rating
5. Submit Request → Provide appliance and issue details
6. Track Status → View submitted issues on dashboard
7. Get Service → Assigned technician contacts customer
8. Logout → Exit platform securely
```

### **Technician Workflow**
```
1. Register → Sign up with specialization
2. Login → Authenticate with credentials
3. View Assignments → See all customer issues assigned
4. Review Details → View customer contact info and problem
5. Contact Customer → Use provided contact methods
6. Complete Service → Mark issue as resolved
7. Delete Issue → Remove completed issues
8. Logout → Exit platform
```

### **Product Seller Workflow**
```
1. Register → Sign up as product seller
2. Login → Authenticate with credentials
3. View Orders → See all repair issues in system
4. Manage Stock → Track replacement parts available
5. Coordinate → Work with technicians on parts
6. Track Sales → Monitor product sales through repairs
7. Logout → Exit platform
```

---

## 🚀 Installation & Setup

### **Prerequisites**
- Node.js (v12 or higher)
- MongoDB (local or Atlas)
- npm or yarn
- Browser with JavaScript enabled

### **Installation Steps**

1. **Clone Repository**:
```bash
git clone https://github.com/rakeshkolipakaace/RTP-Techie-Repair.git
cd RTP-Techie-Repair
```

2. **Install Dependencies**:
```bash
npm install
```

3. **Environment Configuration**:
Create `.env` file:
```env
MONGODB_URI=mongodb://localhost:27017/Login-pro
PORT=5000
SESSION_SECRET=yourSecretKey
```

4. **Update Database Connection** (in Backend/config.js):
```javascript
mongoose.connect("mongodb://localhost:27017/Login-pro")
```

Or use MongoDB Atlas:
```javascript
mongoose.connect("mongodb+srv://user:password@cluster.mongodb.net/Login-pro")
```

5. **Start Server**:
```bash
node Backend/index.js
```

6. **Access Application**:
```
Open browser → http://localhost:5000
```

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| **Total Files** | 11 files |
| **Backend** | 2 files (939 lines) |
| **Frontend** | 7 EJS templates |
| **Total Lines of Code** | 2000+ lines |
| **Database Collections** | 2 (Users, Issues) |
| **Routes** | 20+ API endpoints |
| **User Roles** | 3 (Customer, Technician, Seller) |

---

## 🎓 Key Implementation Details

### **Password Hashing**
```javascript
// Signup
const hashedPassword = await bcrypt.hash(data.password, 10);

// Login
const isPasswordMatch = await bcrypt.compare(password, user.password);
```

### **Session Management**
```javascript
// Set session
req.session.user = user;

// Destroy session
req.session.destroy();

// Check session
if (!req.session.user || req.session.user.role !== "customer")
  return res.redirect("/");
```

### **Issue Creation and Assignment**
```javascript
// Customer submits issue
const newIssue = new IssueModel({
  name, contactNumber, email,
  applianceType, brand, issue, date,
  contactMethod, technician  // Pre-selected by customer
});

await newIssue.save();
```

### **Finding Records**
```javascript
// Find user by email
const user = await UserModel.findOne({ email });

// Find technicians
const technicians = await UserModel.find({ role: 'technician' });

// Find issues for technician
const issues = await IssueModel.find({ technician: req.session.user._id });

// Delete issue
await IssueModel.findByIdAndDelete(idToDelete);
```

---

## 🎯 Features Breakdown

### **Complete**
✅ User registration for 3 roles
✅ Secure login with bcrypt
✅ Session management
✅ Role-based dashboards
✅ Issue creation and assignment
✅ Technician selection
✅ Issue tracking
✅ Logout functionality
✅ Error handling
✅ Form validation

### **Potential Enhancements**
- [ ] Email notifications to technician when assigned
- [ ] Real-time notification system
- [ ] Rating and review system for technicians
- [ ] Payment integration
- [ ] Service scheduling with calendar
- [ ] WhatsApp integration for notifications
- [ ] SMS alerts
- [ ] Issue status workflow (Pending, Assigned, In Progress, Completed)
- [ ] File upload for issue images
- [ ] Admin dashboard for system management
- [ ] Analytics and reporting
- [ ] Mobile app

---

## 🔧 Technical Architecture

### **Request Flow**

```
User Request
    ↓
Express Router
    ↓
Session Check (if protected route)
    ↓
Authentication/Validation
    ↓
MongoDB Query
    ↓
Response (render/redirect)
    ↓
Response to User
```

### **Data Flow**

```
Customer Dashboard
    ↓
Fill Issue Form
    ↓
POST /submitRequest
    ↓
Validate Fields
    ↓
Create IssueModel
    ↓
Link to Technician
    ↓
Save to MongoDB
    ↓
Show Success Message
    ↓
Issue appears in Technician Dashboard
```

---

## 📚 Code Examples

### **Customer Signup Process**
```javascript
// GET /csignup - Show form
app.get("/csignup", (req, res) => {
    const message = req.query.message || "";
    res.render("csignup", { message });
});

// POST /csignup - Process signup
app.post("/csignup", async (req, res) => {
    const { name, email, password, phone } = req.body;
    
    // Check if user exists
    const existing = await UserModel.findOne({ email });
    if (existing) {
        return res.render("csignup", 
            { message: "User Already Exists" });
    }
    
    // Hash password and create user
    const hashedPassword = await bcrypt.hash(password, 10);
    const newUser = new UserModel({
        name, email, phone,
        password: hashedPassword,
        role: "customer"
    });
    
    await newUser.save();
    res.render("login", 
        { message: "Account Created Successfully" });
});
```

### **Technician Dashboard**
```javascript
// GET /technician - Show technician dashboard
app.get("/technician", async (req, res) => {
    // Check authentication and role
    if (!req.session.user || 
        req.session.user.role !== "technician") {
        return res.redirect("/");
    }
    
    // Get all issues assigned to technician
    const issues = await IssueModel.find({ 
        technician: req.session.user._id 
    });
    
    res.render("technician", { 
        issues, 
        user: req.session.user,
        message: req.query.message || ""
    });
});
```

### **Issue Submission**
```javascript
// POST /submitRequest - Create new issue
app.post("/submitRequest", async (req, res) => {
    const { name, contactNumber, email, applianceType,
            brand, issue, date, contactMethod, technician } = req.body;
    
    // Validate all fields
    if (!name || !contactNumber || !email || 
        !applianceType || !brand || !issue || 
        !date || !contactMethod || !technician) {
        throw new Error("All fields are required.");
    }
    
    // Create and save issue
    const newIssue = new IssueModel({
        name, contactNumber, email, applianceType,
        brand, issue, date, contactMethod, technician
    });
    
    await newIssue.save();
    
    // Get updated technicians list
    const technicians = await UserModel.find({ 
        role: 'technician' 
    });
    
    res.render("cs", { 
        message: "Your issue has been submitted.",
        technicians,
        user: req.session.user
    });
});
```

---

## 🛠 Troubleshooting

### **MongoDB Connection Error**
- **Solution**: Check MongoDB is running and URI is correct
- **Check**: `mongod` service should be running
- **Verify**: Connection string in config.js

### **Session Not Persisting**
- **Solution**: Check session configuration
- **Verify**: Session secret is set
- **Clear**: Browser cookies if needed

### **Bcrypt Errors**
- **Solution**: Ensure bcrypt is properly installed
- **Command**: `npm reinstall bcrypt`
- **Check**: Node.js version compatibility

### **Email Already Exists Error**
- **Solution**: Use different email for signup
- **Or**: Clear MongoDB collection: `db.users.deleteMany({})`

### **Page Not Found (404)**
- **Solution**: Check route spelling and method (GET/POST)
- **Verify**: File path for EJS templates

---

## 📞 API Documentation

### **Authentication Endpoints**

| Method | Route | Purpose | Auth Required |
|--------|-------|---------|---------------|
| GET | / | Homepage/Login | No |
| GET | /csignup | Customer signup form | No |
| GET | /tsignup | Technician signup form | No |
| GET | /psignup | Seller signup form | No |
| POST | /csignup | Create customer | No |
| POST | /tsignup | Create technician | No |
| POST | /psignup | Create seller | No |
| POST | /login | Authenticate user | No |
| GET | /logout | Destroy session | Yes |

### **Dashboard Endpoints**

| Method | Route | Purpose | Auth Required | Role |
|--------|-------|---------|---------------|------|
| GET | /cs | Customer dashboard | Yes | Customer |
| GET | /technician | Technician dashboard | Yes | Technician |
| GET | /ps | Seller dashboard | Yes | Seller |

### **Data Endpoints**

| Method | Route | Purpose | Auth Required |
|--------|-------|---------|---------------|
| POST | /submitRequest | Submit repair issue | Yes (Customer) |
| POST | /delete/:id | Delete issue | Yes (Technician) |

---

## 🌟 Best Practices Demonstrated

✅ **Security**: Password hashing with bcrypt
✅ **Authentication**: Session-based auth with role-based access
✅ **Database**: MongoDB with Mongoose schemas
✅ **Error Handling**: Try-catch blocks and validation
✅ **Code Organization**: Separation of concerns (Backend/Frontend)
✅ **Routing**: Clean and organized Express routes
✅ **Views**: EJS templating for dynamic content
✅ **User Experience**: Consistent messaging across pages
✅ **Database Integrity**: Validation before save
✅ **Session Management**: Proper session handling

---

## 📄 License

This project is part of Rakesh's GitHub portfolio for educational purposes.

---

## 🎬 Quick Start

```bash
# 1. Clone
git clone https://github.com/rakeshkolipakaace/RTP-Techie-Repair.git
cd RTP-Techie-Repair

# 2. Install dependencies
npm install

# 3. Configure MongoDB
# Update Backend/config.js with your MongoDB URI

# 4. Start server
node Backend/index.js

# 5. Open browser
# Navigate to http://localhost:5000

# 6. Test platform
# Sign up as customer, technician, or seller
# Submit repair requests
# Track issues in respective dashboards
```

---

## 🏆 Learning Outcomes

After studying this project, you will understand:

✅ Full-stack web development
✅ Express.js server setup and routing
✅ MongoDB database design and queries
✅ Mongoose schema modeling
✅ Password security with bcrypt
✅ Session-based authentication
✅ Role-based access control
✅ EJS templating
✅ Form handling and validation
✅ Error handling in Node.js
✅ REST API design
✅ Multi-user platform development

---

**Created by**: Rakesh  
**Repository**: https://github.com/rakeshkolipakaace/RTP-Techie-Repair  
**Type**: Full-Stack Web Application  
**Stack**: Node.js, Express, MongoDB, EJS  
**Last Updated**: 2024

---

**Build Complete Service Platforms! Master Full-Stack Development! 🚀**
