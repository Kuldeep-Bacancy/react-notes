# React Router Notes

- react-router-dom library will use for routing in react.
- There are two tags we can use for creating href tag.We can't use normal a tag because it refresh the whole page.

 1. Link
 2. NavLink

```javascript
<Link to="/" className="flex items-center"> 
  <img
    src="https://alexharkness.com/wp-content/uploads/2020/06/logo-2.png"
    className="mr-3 h-12"
    alt="Logo"
  />
</Link>

<NavLink
  to="/"
  className={({isActive}) =>
      `block py-2 pr-4 pl-3 duration-200 ${isActive ? "text-orange-700" : "text-gray-700"} border-b border-gray-100 hover:bg-gray-50 lg:hover:bg-transparent lg:border-0 hover:text-orange-700 lg:p-0`
  }
>
  Home
</NavLink>
```

- The difference between Link and Navlink is that we get **isActive** variable when we create link using Navlink.

- Then in main.jsx file we don't need to render App we need to provide RouterProvider and prop route which load the routes. You need to import **RouterProvider**.

```javascript
<React.StrictMode>
  <RouterProvider router={router}/>
</React.StrictMode>
```

-There are two ways you can create routes.

1.

```javascript
const router = createBrowserRouter([
  {
    path: '/',
    element: <Layout />,
    children: [
      {
        path: "",
        element: <Home />
      }, 
      {
        path: "about-us",
        element: <About />
      },
      {
        path: "contact-us",
        element: <Contact />
      }
    ]  
  }
])
```

2.

```javascript
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path='/' element={<Layout />}  errorElement={<Error />}>
      <Route path="" element={<Home />} />
      <Route path="about-us" element={<About />} />
      <Route path="contact-us" element={<Contact />} />
      <Route path="users/:userid" element={<User />}/>
    </Route>
  )
)
```

- Special to note that We have used ```<Layout />``` and ```<Error />``` components.

- So In Layout component we have used ```<Outlet />```. So outlet will inject all child routes of your parent routes.

- So outlet will inject Home, About, Contact and User component between Header and Footer when we click on any link.  

```javascript
import React from 'react'
import Header from './components/Header/Header'
import Footer from './components/Footer/Footer'
import { Outlet } from 'react-router-dom'

function Layout() {
  return (
    <>
      <Header />
      <Outlet />
      <Footer />
    </>
  )
}

export default Layout
```

- Error componet will inject when we enter url which is not mentioned in Routes.

```javascript
import React from 'react'
import { useRouteError } from "react-router-dom";

function Error() {
  const error = useRouteError();

  return (
    <div id="error-page" className='text-center'>
      <h1>Oops!</h1>
      <p>Sorry, an unexpected error has occurred.</p>
      <p>
        <i>{error.statusText || error.message}</i>
      </p>
    </div>
  );
}

export default Error
```

- We will use useRouteError hook to display error message.

- If we want to create dynamic routes which id we can craeate like below.

```javascript
<Route path="users/:userid" element={<User />}/>
```

- Then in component we can get that parameter using *useParams* hook.

```javascript
import React from 'react'
import { useParams } from 'react-router-dom'

function User() {

  const { userid } = useParams()
  return (
    <div>User: {userid} </div>
  )
}

export default User
```

- We can use **loader** attribute in ```<Route />``` if we want to fetch data when we are clicking on link to render component.

```javascript
import Github, { githubInfoLoader } from './components/Github/Github.jsx'

<Route 
  loader={githubInfoLoader}
  path='github' 
  element={<Github />}
/>
```

- Then in component we can use useLoaderData to get the data.

```javascript
import React, { useEffect, useState } from 'react'
import { useLoaderData } from 'react-router-dom'

function Github() {
    const data = useLoaderData()
    // const [data, setData] = useState([])
    // useEffect(() => {
    //  fetch('https://api.github.com/users/hiteshchoudhary')
    //  .then(response => response.json())
    //  .then(data => {
    //     console.log(data);
    //     setData(data)
    //  })
    // }, [])
    
  return (
    <div className='text-center m-4 bg-gray-600 text-white p-4 text-3xl'>Github followers: {data.followers}
    <img src={data.avatar_url} alt="Git picture" width={300} />
    </div>
  )
}

export default Github

export const githubInfoLoader = async () => {
    const response = await fetch('https://api.github.com/users/hiteshchoudhary')
    return response.json()
}
```
