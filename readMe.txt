1. Context API:
    Clean and easy way to share state between components, 
    It is just like Vuex. 
    It is good to use Context API in situations such as:
      Current authenticated user, theme, or preferred languages

    [ATTENTION]
    Context is for global state that shared with many different 
    components.

2. Hooks: 
    hooks are special functions and allow us to do additional
    things inside functional components that we normally use 
    inside class components 

3. We can make context component for each type of data that we 
  want to use, in order to do this: 

  import React, {Component, createContext} from 'react';

  // making the context
  export const ThemeContext =  createContext();
  // making the component for wrapping the elements that 
  // they want to use this data 
  class ThemeContextProvider extend Component {
    state = {
      // data 
    }
    render() {
      return(
        <ThemeContext.Provider value={{...this.state}}>
          // The value property define the data that we want to 
          // give the children
          {this.props.children}  // When we wrap the element,
          // we get this elements in props of the component 
        </ThemeContext.Provider>
      )
    } 
  }
  expert default ThemeContextProvider;


  4. To use context data in the children component, For class based
      componets: 

  // pay attention that we import ThemeContext not ThemeContextProvider
      import {ThemeContext} from './contexts/ThemeContext'

      class Navar extend Component {
        // by using this, the ThemeContext data will be added to
        // this.context
        static contextType = ThemeContext;
        render() {
          console.log(this.context)
          return()
        }
      }

5. To use context data in the children component, and you can use 
  this way in only functional components:
  
  import React from 'react'
  import {ThemeContext} from '../contexts/ThemeContext';

  const Navbar = () => {
    return(
      <ThemeContext.Consumer>{(context) => {
        const {isLightTheme, light, dark} = context;
        const theme = isLightTheme ? light : dark;
        return (
          <nav style={{ background: theme.ui, color: theme.syntax }}>
            <h1>Context app</h1>
            <ul>
              <li>Home</li>
              <li>About</li>
              <li>Contact</li>
            </ul>
          </nav>
        ); 
      }}</ThemeContext.Consumer>
    );
  }

6. We can pass functions in value property of ThemeContext.Provider,
  and use this function, just like we use data:

    export const ThemeContext = createContext();
    class ThemeContextProvider extends Component {
      state = { 
        isLightTheme: true,
        light: { syntax: '#555', ui: '#ddd', bg: '#eee'},
        dark: { syntax: '#ddd', ui: '#333', bg: '#555'}
      }
      toggleTheme = () => {
        this.setState({isLightTheme: !this.state.isLightTheme});
      }
      render() { 
        return ( 
          <ThemeContext.Provider value={{...this.state, toggleTheme: this.toggleTheme}}>
            {this.props.children}
          </ThemeContext.Provider>
        );
      }
    }

    export default ThemeContextProvider;


7. In order to use multiple context, we can use multiple 
  Context.Consumer:

const Navbar = () => {
  return (
    <AuthContext.Consumer>{(authContext) => {
      return (
        <ThemeContext.Consumer>
          {(themeContext) => {
            const {isAuthenticated, toggleAuth} = authContext;
            const { isLightTheme, light, dark } = themeContext;
            const theme = isLightTheme ? light : dark;
            return (
              <nav style={{ background: theme.ui, color: theme.syntax }}>
                <h1>Context app</h1>
                <div onClick={toggleAuth}>
                  {isAuthenticated ? 'Logged in' : 'Logged out'}
                </div>
                <ul>
                  <li>Home</li>
                  <li>About</li>
                  <li>Contact</li>
                </ul>
              </nav>
            );
          }}</ThemeContext.Consumer>
      );
    }}</AuthContext.Consumer>
  );
}
export default Navbar

8. Hooks: 
    hooks are just special functions and allow us to do additional
    things inside functional components, that we normally use 
    inside class components 
