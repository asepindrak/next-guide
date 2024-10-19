# NextGuide (AI for NextJS & HonoJS)

```
NextGuide is an AI assistant that helps developers build apps using Next.js, Hono.js, Docker, and other technologies. It offers insights on state management, React Query, ZOD validation, unit testing, and logging, ensuring effective development 🚀✨
```

```
Hello! I'm NextGuide, your friendly AI assistant specialized in frontend development using Next.js and backend 
development using Hono.js. My main purpose is to assist developers like you by providing insights and guidance on the
technologies and frameworks you mentioned. Whether it's helping with state management, setting up a backend server, or
dealing with JWT validation, I'm here to help!
```


## Installation
1. Install ollama
2. `ollama run adens/next-guide`


## Sample AI Response
>>> Please create a nextjs code for a form with input fields for name, date of birth, address, and email with validation; gender
...  options for Male and Female; password with confirmation; and a submit button with validation


 Sure, here is an example of how you can build the described form using Next.js, TypeScript, Shadcn-ui, React Hook Form,        
Zod, and react-hook-form/resolvers.

Firstly, install required packages:

```bash
npm install @shadcn/ui @types/react-dom react-dom react react-hook-form zod @hookform/resolvers
```

Here is the form component code (`Form.tsx`):

```jsx
"use client";

import { useForm } from "react-hook-form";
import * as z from "zod";
import { zodResolver } from "@hookform/resolvers/zod";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group";
import { Form, FormControl, FormDescription, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form";
import { format } from "date-fns";
import ReactDatePicker from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

const formSchema = z.object({
  name: z.string().min(1, "Name is required."),
  dob: z.date({ required_error: "Date of birth is required." }),
  address: z.string().min(1, "Address is required."),
  email: z.string().email("Invalid email"),
  gender: z.enum(["Male", "Female"], { required_error: "Gender is required." }),
  password: z.string().min(8, "Password must be at least 8 characters long"),
  confirmPassword: z.string(),
}).refine((data) => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"],
});

type FormSchema = z.infer<typeof formSchema>;

export function RegistrationForm() {
  const form = useForm<FormSchema>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      name: "",
      dob: new Date(),
      address: "",
      email: "",
      gender: "Male",
      password: "",
      confirmPassword: "",
    },
  });

  function onSubmit(data: FormSchema) {
    console.log("Submitted data", data);
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Name</FormLabel>
              <FormControl>
                <Input placeholder="Enter your name" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Date of birth field */}
        <FormField
          control={form.control}
          name="dob"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Date of Birth</FormLabel>
              <FormControl>
                <ReactDatePicker
                  onChange={field.onChange}
                  selected={field.value}
                  peekNextMonth
                  showMonthDropdown
                  showYearDropdown
                  dropdownMode="select"
                  placeholderText="Select date of birth"
                  className="form-control"
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Address field */}
        <FormField
          control={form.control}
          name="address"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Address</FormLabel>
              <FormControl>
                <Input placeholder="Enter your address" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Email field */}
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <FormControl>
                <Input placeholder="Enter your email" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Gender field */}
        <FormField
          control={form.control}
          name="gender"
          render={({ field }) => (
            <FormItem className="space-y-3">
              <FormLabel>Gender</FormLabel>
              <FormControl>
                <RadioGroup onValueChange={field.onChange} defaultValue={field.value}>
                  <div className="flex items-center space-x-2">
                    <RadioGroupItem value="Male" />
                    <label htmlFor="r1">Male</label>
                  </div>
                  <div className="flex items-center space-x-2">
                    <RadioGroupItem value="Female" />
                    <label htmlFor="r2">Female</label>
                  </div>
                </RadioGroup>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Password field */}
        <FormField
          control={form.control}
          name="password"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Password</FormLabel>
              <FormControl>
                <Input placeholder="Enter your password" type="password" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Confirm Password field */}
        <FormField
          control={form.control}
          name="confirmPassword"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Confirm Password</FormLabel>
              <FormControl>
                <Input placeholder="Confirm your password" type="password" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        <Button type="submit">Submit</Button>
      </form>
    </Form>
  );
}
```

This example uses Zod for schema validation and react-hook-form for handling form state. It includes date picker for date       
of birth, input fields for name, address, email with respective validation rules and a radio group for selecting gender.        
Passwords are validated using Zod's `refine` method which checks if the passwords match. The form is submitted by calling       
`form.handleSubmit(onSubmit)`, where onSubmit is your submit function that gets called once all fields have passed their        
respective validation rules.

Remember to include `@hookform/resolvers` and `react-datepicker` in your package.json using npm:

```bash
npm install @hookform/resolvers react-datepicker --save
```