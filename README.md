# Use-PasswordHasher-In-Class-Ready
Step-by-step tutorial for using the PasswordHasher ready class 

How to use the PasswordHasher class 
-------------------------------------

Convert string or number with Hash :

public ResultRegisterUsersDto Execute(RequestRegisterUsersDto request)
        {
            var passwordHasher = new PasswordHasher();
            var hashedPassword = passwordHasher.HashPassword(request.Password); //User input 
            User user = new()
            {
                Email = request.Email,
                FullName = request.FullName,
                Password = hashedPassword, //Send to database 
            };

            _context.Users.Add(user);
            _context.SaveChanges();

            return new ResultRegisterUsersDto()
            {
                //-------------
            };
        }
        
----------------------------------------
How to compare the user's password with the password stored in the database 

public ResultLoginUserDto Execute(RequestLoginUserDto request)
        {
            var user = _context.Users
                .Where(p => p.Email.Equals(request.Email))
                .FirstOrDefault();

            var passwordHasher = new PasswordHasher();
            bool resultVerifyPassword = passwordHasher.VerifyPassword(user.Password, request.Password);
            if (!resultVerifyPassword)
            {
                return new ResultLoginUserDto
                {
                    Data = new ResultLoginUserDto { },
                    IsSuccess = false,
                };
            }

            return new ResultLoginUserDto
            {
                IsSuccess = true,
                Message="",
            };

        }
