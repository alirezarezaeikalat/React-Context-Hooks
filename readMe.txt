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

9. We can use useState hooks in functional components:
    useState is a function that return data, and setState function

      import React, { useState } from 'react';
      import uuid from u
      const SongList = () => {
        const [songs, setSongs] = useState([
          { title: 'almost home', id: 1 },
          { title: 'memory gospel', id: 2 },
          { title: 'this wild darkness', id: 3 },
        ]);
        const addSong = () => {
          setSongs([...songs, {title: 'new song', id: 4}]);
        }
        return ( 
          <div className="song-list">
            <ul>
              {songs.map(song => {
                return (
                <li key={ song.id }>{ song.title }</li>
                )
              })}
            </ul>
            <button onClick={addSong}>Add a song</button>
          </div>
        );
      }
      
      export default SongList;

10. useEffect is just like life cycle hooks in class components,
    it runs every time the component render to the dom:

it runs by every data changes:

    import React, { useState, useEffect } from 'react';
    import uuid from 'uuid/v1';
    import NewSongForm from './NewSongForm';
    const SongList = () => {
      const [songs, setSongs] = useState([
        { title: 'almost home', id: 1 },
        { title: 'memory gospel', id: 2 },
        { title: 'this wild darkness', id: 3 },
      ]);
      const addSong = (title) => {
        setSongs([...songs, {title: title, id: uuid()}]);
      }

      useEffect(() => {
        console.log('useEffect hook ran', songs)
      })
      return ( 
        <div className="song-list">
          <ul>
            {songs.map(song => {
              return (
              <li key={ song.id }>{ song.title }</li>
              )
            })}
          </ul>
          <NewSongForm addSong={addSong}/>
        </div>
      );
    }
    
    export default SongList;

[ATTENTION]
  we can specify dependency for the useEffect hooks (in case of the 
  change in the specified data, the useEffect hooks runs)

11. we can use useContext hooks, to use context inside the functional
  components, (without hooks, we use static contextType or 
  context.consumer)

    import React, { useContext } from 'react';

    const BookList = () => {
    const { isLightTheme, light, dark } = useContext(ThemeContext);
    const theme = isLightTheme ? light : dark; 
    return (
      <div
        className="book-list"
        style={{ color: theme.syntax, background: theme.bg }}
      >
        <ul>
          <li style={{ background: theme.ui }}>The way of the kings</li>
          <li style={{ background: theme.ui }}>The name of the wind</li>
          <li style={{ background: theme.ui }}>The final empire</li>
        </ul>
      </div>
    );
  }
  export default BookList; 

12. We can also makes context with, hooks, we only use class based 
    components to use the state in that class based


13. We can use reducers with context and hooks: 

    a. first make the reducer function, in fact the reducer function
      is the function that we pass to the special hook, and we give this
      function(reducer) state and action, each action can have the payload
      beside the type:

        import { v4 as uuidv4 } from "uuid";
        export const bookReducer = (state, action) => {
          switch(action.type){
            case 'ADD_BOOK':
              return [...state, {
                title: action.book.title, 
                author: action.book.author,
                id: uuidv4() 
              }]
            case 'REMOVE_BOOK':
              return state.filter(book => book.id !== action.id);
            default:
              return state;
          }
        }

      b. then we should useReducer in context instead of the useState:

        import React, { createContext, useReducer } from 'react';
        import { bookReducer } from '../reducers/BookReducer';
        
        export const BookContext = createContext();
        const BookContextProvider = (props) => {
          // We begin by the empty array for the books
          const [books, dispatch] = useReducer(bookReducer, []);
          return ( 
            <BookContext.Provider value={{books, dispatch}}>
              { props.children }
            </BookContext.Provider>
          );
        }
        export default BookContextProvider;




    